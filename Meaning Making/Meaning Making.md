```dataviewjs
const folder = "01 PhD Core Content/02 Literature/Zotero";
const headerText = "Magenta — Meaning Making";
const colourMarker = "## <span style=\"color:";
const pages = dv.pages(`"${folder}"`);

for (let page of pages) {
  const content = await dv.io.load(page.file.path);
  if (!content || !content.includes(headerText)) continue;

  const headerIndex = content.indexOf(headerText);
  if (headerIndex === -1) continue;

  const currentColourStart = content.lastIndexOf(colourMarker, headerIndex);
  if (currentColourStart === -1) continue;

  const nextColourIndex = content.indexOf(colourMarker, currentColourStart + 1);

  const sectionStart = content.indexOf("\n", currentColourStart);
  if (sectionStart === -1) continue;

  const sectionEnd = nextColourIndex === -1 ? content.length : nextColourIndex;

  let section = content.slice(sectionStart + 1, sectionEnd);

  let lines = section.split("\n").filter(line =>
    !line.toLowerCase().includes("linked hub")
  );

  section = lines.join("\n").trim();
  if (!section) continue;

  dv.header(3, page.file.name);
  dv.paragraph(section);
}

```
