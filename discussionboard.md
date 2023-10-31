---
comments: false
layout: default
title: Discussion Board
permalink: /discussionboard
---

<html>
<table>
  <thead>
  <tr>
    <th>Joke</th>
    <th>HaHa</th>
    <th>Boohoo</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>
<script>
// prepare HTML defined "result" container for new output
const resultContainer = document.getElementById("result");
// keys for joke reactions
const HAHA = "haha";
const BOOHOO = "boohoo";
// prepare fetch urls
const url = "https://flask.nighthawkcodingsociety.com/api/jokes";
const like_url = url + "/like/";  // haha reaction
const jeer_url = url + "/jeer/";  // boohoo reaction
// prepare fetch GET options
const options = {
  method: 'GET', // *GET, POST, PUT, DELETE, etc.
  mode: 'cors', // no-cors, *cors, same-origin
  cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
  credentials: 'omit', // include, *same-origin, omit
  headers: {
    'Content-Type': 'application/json'
    // 'Content-Type': 'application/x-www-form-urlencoded',
  },
};
// prepare fetch PUT options, clones with JS Spread Operator (...)
const put_options = {...options, method: 'PUT'}; // clones and replaces method
// fetch the API
fetch(url, options)
  // response is a RESTful "promise" on any successful fetch
  .then(response => {
    // check for response errors
    if (response.status !== 200) {
        error('GET API response failure: ' + response.status);
        return;
    }
    // valid response will have JSON data
    response.json().then(data => {
        console.log(data);
        for (const row of data) {
          // make "tr element" for each "row of data"
          const tr = document.createElement("tr");        
          // td for joke cell
          const joke = document.createElement("td");
            joke.innerHTML = row.id + ". " + row.joke;  // add fetched data to innerHTML
          // td for haha cell with onclick actions
          const haha = document.createElement("td");
            const haha_but = document.createElement('button');
            haha_but.id = HAHA+row.id   // establishes a HAHA JS id for cell
            haha_but.innerHTML = row.haha;  // add fetched "haha count" to innerHTML
            haha_but.onclick = function () {
              // onclick function call with "like parameters"
              reaction(HAHA, like_url+row.id, haha_but.id);  
            };
            haha.appendChild(haha_but);  // add "haha button" to haha cell
          // td for boohoo cell with onclick actions
          const boohoo = document.createElement("td");
            const boohoo_but = document.createElement('button');
            boohoo_but.id = BOOHOO+row.id  // establishes a BOOHOO JS id for cell
            boohoo_but.innerHTML = row.boohoo;  // add fetched "boohoo count" to innerHTML
            boohoo_but.onclick = function () {
              // onclick function call with "jeer parameters"
              reaction(BOOHOO, jeer_url+row.id, boohoo_but.id);  
            };
            boohoo.appendChild(boohoo_but);  // add "boohoo button" to boohoo cell 
          // this builds ALL td's (cells) into tr (row) element
          tr.appendChild(joke);
          tr.appendChild(haha);
          tr.appendChild(boohoo);
          // this adds all the tr (row) work above to the HTML "result" container
          resultContainer.appendChild(tr);
        }
    })
})
// catch fetch errors (ie Nginx ACCESS to server blocked)
.catch(err => {
  error(err + " " + url);
});
</script>
</html>