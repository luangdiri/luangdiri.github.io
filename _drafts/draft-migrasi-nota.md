---
title: "Migrasi Nota"
date: 2024-02-03 16:01:01
tags:
- meta
---

# Migrasi Nota

2024-02-03 15:16:15

notes to md
- standard-notes (plaintext)
- evernote (batch pandoc)
- gnotes (batch ren move pandoc)
- juno diary (js)
- imaskingforit blog (blog2md)
- blogspot (blog2md)
- knowledgelog blog (ghost2md)
- aemxn blog (static site)

Script written assisted by ChatGPT.

Batch Convert HTML to Markdown using Pandoc:

```bat
@echo off

rem Loop through each HTML file and convert to Markdown
for %%i in (*.html) do (
    pandoc "%%i" --from html --to markdown_strict -o "%%~ni.md"
)
```

`"%%~ni"`: get the filename only, removes extension

GNotes structure

```
D:.
│   html2md.bat
│   rename.bat
├───5758c3a843b8b46ba2444776
│       5758c3a843b8b46ba2444776.html
│
├───5759d18d2f2e8d8043eb9dc9
│       5759d18d2f2e8d8043eb9dc9.html
│
├───57b0ac1cf8d0db34d6a357ef
│       57b0ac1cf8d0db34d6a357ef.html
│
├───760c2931-0910-40cb-a112-38835bdefc6a
│       760c2931-0910-40cb-a112-38835bdefc6a.html
│
└───f7e21221-4473-474c-8751-cbdcefb2bebf
        f7e21221-4473-474c-8751-cbdcefb2bebf.html
```

Rename and move script. Pandoc done separately for control:

```bat
@echo off
setlocal enabledelayedexpansion

set "sourceFolder=%cd%"

for /d %%i in ("%sourceFolder%\*") do (
    set "folderName=%%~nxi"
    set "fileName=content.html"
    
    set "newFileName=!folderName!.html"
    
    ren "%%i\!fileName!" "!newFileName!"
    
    copy "%%i\!newFileName!" "%sourceFolder%"
)

echo Done!
```

Sample JSON diary structure:

```json
{
  "entries": [
    {
      "id": 1,
      "title": "From OhLife (November 28, 2014)",
      "date": "2014-12-07",
      "body": "\\nkajfsdbfkjabsdfkjadbsfkjasdnfknasdfk.\\nisandfkja we\\'re sndfkjasndf. aksjdnfkasndfkjadsnf.",
      "createdAt": "2020-07-11T22:04:14.000Z",
      "updatedAt": "2020-07-11T22:04:14.000Z"
    },
    {
      "id": 2,
      "title": "Big Bad Dream",
      "date": "2014-12-08",
      "body": "Lorem ipsum dolor sit amet, ....",
      "createdAt": "2020-07-11T22:04:14.000Z",
      "updatedAt": "2020-07-11T22:04:14.000Z"
    },
    {
      "id": 2999,
      "title": "AD 2172.2023",
      "date": "2023-03-16",
      "body": "The standard chunk of...",
      "createdAt": "2023-03-16T17:19:49.000Z",
      "updatedAt": "2023-03-16T17:19:49.000Z"
    }
  ]
}
```

Juno diary javascript:

```javascript
const fs = require('fs');

// Read JSON data from external file
const jsonData = JSON.parse(fs.readFileSync('sample.json', 'utf-8'));

// Iterate over each entry and store formatted content in a separate Markdown file
jsonData.entries.forEach(entry => {
    const originalTitle = entry.title;
    const date = new Date(entry.date);
    const body = unescape(entry.body); // Unescape the body field

    // Format the date as "DD.MM.YYYY"
    const day = String(date.getDate()).padStart(2, '0');
    const month = String(date.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed in JavaScript
    const year = date.getFullYear();
    const formattedDate = `${day}.${month}.${year}`;

    // Create formatted title as "AD YYYY.MM.DD"
    const formattedTitle = `AD ${formattedDate}`;

    // Create a folder for the year and month
    const folderPath = `./${year}/${month}`;

    if (!fs.existsSync(folderPath)) {
        fs.mkdirSync(folderPath, { recursive: true });
        console.log(`Created folder: ${folderPath}`);
    }

    // Use formatted title as the filename with .md extension
    const filename = `${folderPath}/${formattedTitle}.md`;

    // Write Markdown content to file
    const markdownContent = `# ${originalTitle}\n\nDate: ${formattedDate}\n\n${body}`;

    // Remove '\n' characters from the body
    const cleanedMarkdown = markdownContent.replace(/\\n/g, ' ').replace(/\\'/g, '\'');

    fs.writeFileSync(filename, cleanedMarkdown, 'utf-8');

    console.log(`Markdown file created: ${filename}`);
});

```

TODO: telegram export to MD