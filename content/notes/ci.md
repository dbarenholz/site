---
title: "CI/CD setup"
description: "How I manage CI/CD with Crow CI and copious amounts of proxying."
date: "2026-07-12"
taxonomies:
  tags: ["homelab", "networking"]
---

[TOC]

There are many possible solutions for **continuous integration and continuous delivery/deployment** ("CI/CD").
Those on GitHub can use [GitHub Actions](https://github.com/features/actions).
[Codeberg](https://docs.codeberg.org/ci/actions/), which is based on [Forgejo](https://forgejo.org/docs/latest/user/actions/reference/), also has _Actions_.
People using GitLab have [Pipelines](https://docs.gitlab.com/ci/pipelines/), and those giving money to Atlassian can use [Pipelines](https://www.atlassian.com/software/bitbucket/features/pipelines) as well.
Cloud providers like [Azure](https://learn.microsoft.com/en-us/azure/devops/pipelines/architectures/devops-pipelines-baseline-architecture?view=azure-devops) and [GCP](https://cloud.google.com/blog/topics/developers-practitioners/devops-and-cicd-google-cloud-explained) have their own solutions as well.
Clearly, people care about CI/CD.
If I look at _standalone_ software I can run on my own hardware, then there's [an even larger set of options](https://github.com/awesome-foss/awesome-sysadmin#continuous-integration--continuous-deployment) to choose from[^1].
Forgejo Actions is one of them, for instance, but there's also [Drone](https://docs.drone.io/), [Jenkins](https://www.jenkins.io/), and a bunch more.
And yet, the one I chose isn't linked in this post (yet!), nor is it on the linked large set of options.
I run [Crow CI](https://crowci.dev/v5-13/), a fork of [Woodpecker CI](https://woodpecker-ci.org/) (you needed more options, right?).

The reason I'm on crow is because it's a "container only" application.
According to their docs, other solutions as linked above generally execute "host-first", namely, pipelines typically run directly on a VM and thus are limited by the OS of that VM.
With crow you can run in any (docker/podman/kubernetes) container, so you can easily test on [virtually](https://hub.docker.com/_/archlinux/) [all](https://hub.docker.com/_/ubuntu) [the](https://hub.docker.com/_/alpine) [linux](https://hub.docker.com/_/fedora) [flavours](https://hub.docker.com/r/redhat/ubi10) [you'd](https://hub.docker.com/_/debian) [like](https://hub.docker.com/r/gentoo/stage3) (yes these are all options), as well as [windows](https://hub.docker.com/r/microsoft/windows) and [macos](https://github.com/sickcodes/Docker-OSX).
Did I mention it works with any git forge (concurrently), and is relatively lightweight in terms of memory consumption?

More concretely, you can tell crow to spin up a database or other service during your tests.
Like many other CI's, crow uses `yaml` (also supports more complex stuff, it's cool software) to specify the pipeline/action/services.
If your program depends on a postgres database and minio being available, and you want those in CI for end to end testing, it's as easy as just.. declaring them.

```yaml
services:
- name: postgres-db
  image: postgres:18-alpine
  environment:
    POSTGRES_DB: my-database
    POSTGRES_USER: my-database-user
    POSTGRES_HOST_AUTH_METHOD: trust
- name: minio
  image: minio/minio:latest
  commands:
    - minio server /data
```

If the thing you make is a containerized application like [my wishlist app Omiyage](@/projects/omiyage.md) because you get annoyed at family members asking what you want for your birthday (such a luxury problem, I know), then even better.
Run e2e (end to end) tests directly with crow by spinning up the frontend and backend services, together with the database/redis/minio/... it needs.
Because crow runs containers, this is easy.
Then the e2e test becomes a very dumb bash script, roughly like this:

1. wait for all services to be up as e.g. `while ! curl ...; do sleep X; done`
2. run the e2e tests with e.g. `bun x playwright test`

I thoroughly enjoy working with it.
But this post is not about how to use Crow CI at all.
Instead, this is a technical post about a problem encountered while job hunting.

## the problem

When looking for (tech) jobs, it's generally useful to showcase your skills.
If you have a decent resume/CV with years of work experience, that usually suffices to get you past the _we judge you based on a piece of paper_ phase.
However, if you're switching fields or are otherwise just starting out, you need the dreaded portfolio.
It's so bad, that there's even a [github repo](https://github.com/emmabostian/developer-portfolios) with over 1000 different dev portfolios linked.
For such portfolio it's pretty useless if all your projects live on your own git server behind VPN access.
Even more, if your CI also runs behind VPN access, then projects on GitHub won't be able to use your CI!

Thus, the problem: **I need a way to run CI on my own server, in my safe environment, such that it can talk to my own forge, but _also_ have it accessible from the outside for both github and recruiters (not) to peek at**.

## the solution: a rube goldberg machine[^2]

> "... intentionally designed to perform a simple task in a comically overcomplicated way ..."

The Crow CI quickstart itself is straightforward: copy paste a compose file, set the version number to use, then run `docker compose up` and call it a day.
However, I need to solve the above problem, and make sure that it can access the things I want and is accessible by the big scary internet in a safe enough fashion.
As I explain in [my site hosting post](@/notes/site-hosting.md), this site is hosted on a raspberry pi in a separate VLAN using a DMZ.
I want to use that pi as a one-stop-shop for all incoming connections.
So, I run CI on my main network, then proxy from my pi to that main network (or rather, specifically the crow container running on it).
To access to the pi from the outside world, I use a Cloudflare tunnel (as explained in the linked post): when accessing the public endpoint of my CI, which is [ci.dbarenholz.com](https://ci.dbarenholz.com), you _first_ get proxied by cloudflare to my pi, which _then_ uses Caddy to proxy you through a tiny hole in my firewall to a docker container that appears as real device thanks to [macvlan networking](https://docs.docker.com/engine/network/drivers/macvlan/) which I use solely so containers show up as real devices on my [UX7](https://techspecs.ui.com/unifi/cloud-gateways/ux7) router.

So when I say the solution is a rube goldberg machine, I'm not kidding.
I wonder how many more proxies I can get away with before things break!

## i want this too! (why would you)

If, for some god unknown reason, you'd like to replicate my setup, then here's roughly what to do and what to know.

- Use any router with local DNS so you don't have to memorize as many IP addresses. I have a Unifi UX7, but I'm fairly certain any OpenWRT router can do everything too.
- Use macvlan networking for your homelab setup, so docker containers show up in the firewall interface of the router as devices.
- Use a dedicated box (my case: raspbery _pi_) for incoming connections and publically hosted stuff; have it live in a dedicated VLAN.
- Besides the dedicated box, have _another_ machine (or more! I won't judge. Promise.) on which you run your actual services. In my case, this is called _server_.

### run crow

With those points out of the way, get Crow CI running on _server_ using macvlan networking by modifying the provided compose file.
In below snippet I use an `.env` file accompanying it; you can choose to type all secrets directly into the compose file instead... but I assume that if you decide to go this route, you probably want to use a separate `.env` file.
Oh, and make sure to set your IPv4 and MAC addresses accordingly for your network.

```yaml
services:
  server:
    image: codefloe.com/crowci/crow-server:v5.12 # choose which version to run; I'm on 5.12 for now
    volumes:
      - ./crow-server:/var/lib/crow
    networks:
      home:
        ipv4_address: '192.168.1.217'
        mac_address: '02:42:C0:A8:01:D9'
      crowci:
    environment:
      - CROW_OPEN=true       # allow registrations; this is set to false once things work
      - CROW_SERVER_ADDR=:80 # set to port 80
      - CROW_FORGEJO=true    # main account from forgejo
      - CROW_ADMIN=dan       # NOTE: you probably have a different username than I do!

      - CROW_AGENT_SECRET=${CROW_AGENT_SECRET}
      - CROW_HOST=${CROW_HOST}
      - CROW_FORGES=${CROW_FORGES}
      - CROW_FORGE_GH_TYPE=${CROW_FORGE_GH_TYPE}
      - CROW_FORGE_GH_CLIENT=${CROW_FORGE_GH_CLIENT}
      - CROW_FORGE_GH_SECRET=${CROW_FORGE_GH_SECRET}
      - CROW_FORGE_FORGEJO_TYPE=${CROW_FORGE_FORGEJO_TYPE}
      - CROW_FORGE_FORGEJO_URL=${CROW_FORGE_FORGEJO_URL}
      - CROW_FORGE_FORGEJO_CLIENT=${CROW_FORGE_FORGEJO_CLIENT}
      - CROW_FORGE_FORGEJO_SECRET=${CROW_FORGE_FORGEJO_SECRET}
  agent:
    image: codefloe.com/crowci/crow-agent:v5.12
    restart: always
    depends_on:
      - server
    networks:
      crowci:
    volumes:
      - ./crow-agent:/etc/crow
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - CROW_SERVER=server:9000
      - CROW_AGENT_SECRET=${CROW_AGENT_SECRET}
networks:
  crowci:
    driver: bridge
    internal: true
  home:
    external: true
    name: home
```

#### aside: mac address generation for macvlan

I chose to do [this](https://stackoverflow.com/questions/42946453/how-does-the-docker-assign-mac-addresses-to-containers) for mac addresses: if your IP address is `a.b.c.d`, then the corresponding MAC address is `02:42:a:b:c:d`, with the digits represented in hex.
I quite like it this way; `02` at the start indicates a locally administered MAC address and the IP can technically be derived from the MAC address.
If you'd like to do it this way too, you can use my `mac2ip.sh` script shown below.

```bash
#!/bin/bash

# Generates a mac address from a given IPv4 address.
function generate_mac_addr() {
	local ip="$1"
	local mac="02" # Unicast, locally administered
	local octet
	mac="$mac:42"  # Docker's OUI (since this is for docker containers)
	for i in {1..4}; do
		octet=$(echo "$ip" | cut -d '.' -f "$i") # grab octet i
		mac="$mac:$(printf '%02X' "$octet")"     # convert number to hex
	done

	echo "$mac"
}

# Checks if a given string is a valid IPv4 address. Returns 0 if valid, 1 otherwise.
# Taken from https://stackoverflow.com/questions/13777387/check-for-ip-validity
function ip_ok() {
	local ip=${1:-NO_IP_PROVIDED}
	local IFS=.
	read -r -a a <<<"$ip"
	[[ $ip =~ ^[0-9]+(\.[0-9]+){3}$ ]] || return 1
	local quad
	for quad in {0..3}; do
		[[ "${a[$quad]}" -gt 255 ]] && return 1
	done
	return 0
}

# Prints usage information.
function help() {
	echo "ip2mac.sh - Generate a MAC address from an IPv4 address ala docker."
	echo "Usage: $0 <IPv4>"
}

# No argument? Help and exit.
if [ "$#" -ne 1 ]; then
	help
	exit 1
fi

# Argument invalid? Error and exit.
if ! ip_ok "$1"; then
	echo "Error: Invalid IP address '$1'."
	exit 1
fi

# Valid IP provided, generate and print the MAC address
generate_mac_addr "$1
```

### proxy galore

Once crow is running, setup the cloudflare -> pi -> container routing.
This is done with a tunnel on cloudflare's end pointing to a particular port.

```bash
ci.dbarenholz.com --> http://127.0.0.1:8000
```

Note how it goes to port `8000`.
Caddy listens on that port and reverse proxies to the local DNS address of the crow container, which in my case is `ci.db`.

```bash
:8000 {
  reverse_proxy http://ci.db
}
```

Make sure that Caddy gets reloaded after changes with e.g. `systemctl restart caddy`.
Not doing so will most likely result in a 502 Bad Gateway.

### poke firewall hole

With cloudfare pointing to Caddy, and Caddy pointing to the CI container, I need to poke a hole in the firewall.
Open the router interface for firewalls and create a new rule.
Give it a reasonable name like `poke-hole-pi-to-crow-ci`.
The better you choose the name, the less guessing you do later when trying to figure out why the rule exists.
The source should be the _pi_ device with any port[^3].
The action is clearly **allow**, since I want to poke a hole.
The destination is the internal IP of the Crow CI container; this is present in the compose file.
I set it to TCP only and both IP versions (don't care).
Finally enable syslog logging so you can see when a hole is poked through the firewall.

### ... profit?

To ensure that both GitHub (external) and Forgejo (internal) can use the same instance, they need to point to the same URI.
If not, there are not-so-silly warnings and errors about unsafe redirects in the oath flow.
This means that everything must use the proxied URI `ci.dbarenholz.com`, which is also the value set in the `.env` file for `CROW_HOST`.
Only downside is that I visit the proxied `ci.dbarenholz.com` when logging in from `ci.db`, but I suppose you can't win all battles.

As final note: changing `CROW_OPEN=false` in the compose file means nobody new can sign up.
If not connected to the home network, you can't see `git.db` (which is good).
Attempting to go through the GitHub flow returns an error (which is good).
So it seems that, even if exposed, it's reasonable when not set to open.

[^1]: Some of these (also, or exclusively) do GitOps (roughly equivalent to automated "infrastructure as code").
[^2]: Thank you [`@deezy`](https://ndumas.com/) on Discord for telling me this term exists: [Rube Goldberg machines](https://en.wikipedia.org/wiki/Rube_Goldberg_machine).
[^3]: I don't know any better: if you have suggestions, let me know!
