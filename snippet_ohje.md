### Ohje snippettien tekemiseen vscodelle

Muokkaa asetuksiin seuraavat user settingsit:
(File > Preferences > Settings)

```
"editor.tabCompletion": true,
"editor.snippetSuggestions": "top"
```
Ja lisää ensimmäinen user snippet:
(File > Preferences > User snippets > JavaScript)

Muokkaa aukeavaan javascript.json tiedoston sisällöksi seuraava

```
{
  // Place your snippets for JavaScript here. Each snippet is defined under a snippet name and has a prefix, body and
  // description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
  // $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the
  // same ids are connected.
  // Example:
  "console.log": {
    "prefix": "clog",
    "body": [
      "console.log('$1')",
    ],
    "description": "Log output to console"
  }
}
```

Nyt voit kirjoittaa `console.log` kirjoittamalla `c`, `l`, `o`, `g`, `tab`

Editoriin pitäisi täydentyä automaattisesti `console.log('')`
