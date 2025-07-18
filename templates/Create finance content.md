<%*
const basePath = "content/finance"; 
const title = await tp.system.prompt("Please enter a title");
if (!title) {
    throw new Error("User cancelled the prompt.");
}

const tags = await tp.system.prompt("Please enter tags (comma-separated)", "");
const folderPath = `${basePath}/${title}`;

if (!app.vault.getAbstractFileByPath(folderPath)) {
    await app.vault.createFolder(folderPath);
}

await tp.file.move(`${folderPath}/index`);

app.workspace.getLeaf(true).openFile(`${folderPath}/index`);
_%>
---
title: <% title %>
date: <% tp.file.creation_date("YYYY-MM-DDTHH:mm:ssZ") %>
# lastmod:
author: "Suika"
draft: true
description: ""
tags: [<% tags %>]
categories:  [ Finace ]
# series: 
# series_order: 
---


{{< lead >}}

{{< /lead >}}
