<%*
const file = tp.file.find_tfile(tp.file.path(true));
if (!file) return;

const content = await app.vault.read(file);

const newContent = content.replace(`draft: true`, `draft: false`);

await app.vault.modify(file, newContent);

new Notice("Published!");
%>
