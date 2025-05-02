---
prowHeroProperty: "-"
fateHeroProperty: "-"
cosmHeroProperty: "-"
romnHeroProperty: "-"
knowHeroProperty: "-"
---
```dataviewjs
function retrieveForbiddenByOverlord() {
	let location = '"Characters/Overlords/Current Overlord"';
	let overlord = dv.pages(location)[0].file.frontmatter.overlordProperty.replace("'","’");
	
	if (overlord == undefined || overlord == "" || overlord == "-") {
		return [];
	}
	
	let overlordLocation = '"Characters/Overlords/Candidates/' + overlord.replace("’","'") + '"';
	let exclude = dv.pages(overlordLocation)[0].file.frontmatter.excludedProperty;

	if (exclude == undefined || exclude == "") {
		return [];
	}
	
	return exclude.split(',').map(s => s.trim());
}

function retrieveForbiddenByOtherHeroes() {
	let current = dv.current();

	let heroes = [
		current.cosmHeroProperty,
		current.fateHeroProperty,
		current.knowHeroProperty,
		current.prowHeroProperty,
		current.romnHeroProperty
	];
	
	let folders = [
		"Cosmos",
		"Fate",
		"Knowledge",
		"Prowess",
		"Romance"
	]; 
	
	let exclude = [];

	for (var i = 0; i < heroes.length; i++) {
		let hero = heroes[i];
		let folder = folders[i];
		let heroLocation = '"Characters/Heroes/' + folder + '/' + hero + '"';
		
		if (hero != undefined && hero != "" && hero != "-") {
			let front = dv.pages(heroLocation)[0].file.frontmatter;
			let prop = front.excludedProperty;
			
			if (prop == undefined || prop == "") {
				continue;
			}
			
			exclude = exclude.concat(prop.split(','));
		}
	}
	
	return exclude.map(s => s.trim());
}

function fetchOptions(path, filter) {
	let pages = [...dv.pages(path)
					.map(p => p.file.name)
					.filter(n => !filter.contains(n))
					.sort()];
					
	let options = ['-'].concat(pages)
					   .map(p => 'option(' + p + ')')
					   .join(', ');
	
	return options;
}

let filter = retrieveForbiddenByOverlord();
filter = filter.concat(retrieveForbiddenByOtherHeroes());

let cosmOptions = fetchOptions('"Characters/Heroes/Cosmos"', filter);
let fateOptions = fetchOptions('"Characters/Heroes/Fate"', filter);
let knowOptions = fetchOptions('"Characters/Heroes/Knowledge"', filter);
let prowOptions = fetchOptions('"Characters/Heroes/Prowess"', filter);
let romnOptions = fetchOptions('"Characters/Heroes/Romance"', filter);

let row = [
	`\`INPUT[inlineSelect(${cosmOptions}, defaultValue(-)):cosmHeroProperty]\``,
	`\`INPUT[inlineSelect(${fateOptions}, defaultValue(-)):fateHeroProperty]\``,
	`\`INPUT[inlineSelect(${knowOptions}, defaultValue(-)):knowHeroProperty]\``,
	`\`INPUT[inlineSelect(${prowOptions}, defaultValue(-)):prowHeroProperty]\``,
	`\`INPUT[inlineSelect(${romnOptions}, defaultValue(-)):romnHeroProperty]\``,
]

dv.paragraph("Heroes:")
dv.table(["Cosmos","Fate","Knowledge","Prowess","Romance"],[row]);
```

```dataviewjs
function draw(property, folder) {
	if (property == undefined || property == "-" || property == "") {
		return;
	}

	dv.paragraph(folder + ": [[Characters/Heroes/" + folder + "/" + property + "|" + property + "]]");
}

let current = dv.current();

dv.paragraph("## Quick links:");
draw(current.cosmHeroProperty, "Cosmos");
draw(current.fateHeroProperty, "Fate");
draw(current.knowHeroProperty, "Knowledge");
draw(current.prowHeroProperty, "Prowess");
draw(current.romnHeroProperty, "Romance");
```

```dataviewjs
function draw(property, folder) {
	dv.paragraph("### Branded by " + folder)
	if (property == undefined || property == "-" || property == "") {
		dv.paragraph("NONE")
	}
	else {		
	    dv.paragraph("![[Characters/Heroes/" + folder + "/" + property + "]]");
	}
}

let current = dv.current();

dv.paragraph("## Selected Heroes:");
draw(current.cosmHeroProperty, "Cosmos");
draw(current.fateHeroProperty, "Fate");
draw(current.knowHeroProperty, "Knowledge");
draw(current.prowHeroProperty, "Prowess");
draw(current.romnHeroProperty, "Romance");
```
#Character #Hero
