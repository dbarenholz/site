---
title: "Site"
excerpt: "My personal site made with Jekyll."
tag: code
classes: wide
---

My personal site, which you are currently browsing, is made using [Jekyll](https://jekyllrb.com/). 
Jekyll is one of many _site generator (SG)_: it takes a bunch of files, in this case _markdown_ files, and then converts them to a HTML page.
The resulting HTML page can then be hosted virtually anywhere.

The reason why I am using a SG is because creating content in markdown is _easy_. 
It takes very little effort to make a site, and little maintenance.
To read more about the philosophy of Jekyll see its page on the [jamstack](https://jamstack.org/generators/jekyll/) site.

Creating the site was fairly enjoyable.
While most things are supported by the theme I use, [minimal mistakes](https://mmistakes.github.io/minimal-mistakes/), the grid of notes on the [notes] page I have implemented myself.
It uses the [_Liquid_](https://jekyllrb.com/docs/liquid/) templating language mixed with plain HTML code to generate, for each note, an item in a grid.


```html
{% raw %}<div class="grid flex">
  {% for note in site.notes %}
     <div class="notes-item {{ note.tag }}">
        <a class="notes-link" data-toggle="modal" href="{{ site.url }}/{{ note.url }}">
          <h4>{{ note.title }}</h4>
        </a>
        <p> {{ note.excerpt | markdownify }}</p>
     </div>
  {% endfor %}
  <p id="emptygrid" style="display:none"> You have filtered out all posts. It is empty now.</p>
</div>{% endraw %}
```

Filtering is achieved with buttons and custom JavaScript code.
The buttons are created as follows. 
Note the `data-filter` attribute, which is what will be used for filtering purposes.
The `active` class is used to indicate whether or not this particular data-filter value is shown in the grid. 
If it is `active`, then I show the related notes, and with `inactive` I hide them. 

```html
<div class="button-group filter-button-group justify-content-center">
    <a class="btn btn-sm btn-primary active" data-filter=".research">Research</a>
    <a class="btn btn-sm btn-primary active" data-filter=".code">Code</a>
    <a class="btn btn-sm btn-primary active" data-filter=".music">Music</a>
</div>
```

The required JavaScript code is as follows. 
I have commented it in-code directly as to make it easier to parse.

```js
// Create a list of values that we filter on.
let filterValues = []
let elems = document.querySelectorAll('.filter-button-group a.active');
for (const elem of elems)
    // This grabs the data-filter attribute of the buttons made above.
    filterValues.push(elem.getAttribute("data-filter"))

// Loop over all the buttons
const buttons = document.querySelectorAll('.button-group a.btn')
for (const item of buttons) {
    // Define what happens when they are clicked
    item.addEventListener('click', (evt) => {
        // First, grab its data-filter attribute
        const value = item.getAttribute("data-filter")
        // Update its visibility.
        // If it was visible, hide it. If it was hidden, show it.
        updateVisibilityFor(value)

        // If the value to filter on was included in the list of values
        if (filterValues.includes(value)) {
            // Then we remove it from the values to filter on
            filterValues.splice( filterValues.indexOf(value), 1 )
            // And we toggle the active and inactive classes
            item.classList.toggle('active');
            item.classList.toggle('inactive');
        } else {
            // Otherwise, we add it back into the list of values to filter on
            filterValues.push(value)
            // And we toggle the active and inactive classes
            item.classList.toggle('active');
            item.classList.toggle('inactive');
        }

        // After all work is done, check if the grid has become empty.
        checkEmptyGrid()
    })
}

// Helper function to update visibility.
// It works by selecting the DOM elements to toggle visibility for, 
//   and then it sets or unsets its display style accordingly.
function updateVisibilityFor(value) {
    const toggleMe = document.querySelectorAll(`.grid > div${value}`)
    toggleMe.forEach(item => {
        item.style.display = item.style.display == 'none' ? 'unset' : 'none'
    })
}

// Helper function to show (and hide) some text indicating that the grid has been emptied due to no active filters
function checkEmptyGrid() {
    const items = document.querySelectorAll(`.grid > div`)
    // this is the piece of text you see when the grid is empty
    const text = document.querySelector('#emptygrid') 

    // Loop through all items in the grid
    let found = false;
    for (const item of items) {
        // If at least one of them is shown (so its display style does not equal none)
        if (item.style.display != 'none') {
            // Then we don't have to do anything
            found = true
            break
        }
    }

    // Set the display style of the text accordingly
    text.style.display = !found ? 'unset' : 'none'
}
```

The [source code](https://github.com/dbarenholz/site) is available on my github, in case you want to see. 

<style>
pre {
    white-space: pre-wrap;
}
</style>