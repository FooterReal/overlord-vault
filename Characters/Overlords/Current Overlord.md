---
overlordProperty: Estoc
---
```dataviewjs
let pages = [...dv.pages('"Characters/Overlords/Candidates"').map(p => p.file.name).sort()];
let options = ['-'].concat(pages).map(p => 'option(' + p + ')').join(', ');

dv.paragraph(`Overlord: \`INPUT[inlineSelect(${options}, defaultValue(-)):overlordProperty]\``);
```

```dataviewjs
let overlord = dv.current().overlordProperty;

dv.paragraph("## Selected Overlord:");
if (overlord == undefined || overlord == "-" || overlord == "") {
	dv.paragraph("NONE")
}
else {
	
    dv.paragraph("![[Characters/Overlords/Candidates/" + overlord + "]]");
}
```

#Character #Overlord