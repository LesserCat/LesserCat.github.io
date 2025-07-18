<%*
const file = tp.file.find_tfile(tp.file.path(true));
if (!file) return;

const content = await app.vault.read(file);
const newTimestamp = tp.date.now("YYYY-MM-DDTHH:mm:ssZ");

const regex = /^#?\s*lastmod:.*$/m;
const newContent = content.replace(regex, `lastmod: ${newTimestamp}`);

await app.vault.modify(file, newContent);

new Notice("Lastmod timestamp updated!");
%>
