<%*
let prefix = await tp.system.prompt("Prefix for folder search"); 

const searchPattern = new RegExp(`^${prefix}_.*`);
const folders = app.vault.getAllFolders();

let folderWithPrefixPath = null; 
for (const folder of folders) { 
	if (searchPattern.test(folder.path)) { 
		folderWithPrefixPath = folder.path; 
		break; 
	} 
}

if (!folderWithPrefixPath) return;

let title = await tp.system.prompt("File Name");
if (!title) return;
let topic = await tp.system.prompt("Lecture topic");
if (!topic) return;

const d = new Date();

const folderPath = folderWithPrefixPath + topic + "/" + d.getDate() + "-" + (d.getMonth() + 1) + "-" + d.getFullYear(); 

const folderExists = await app.vault.adapter.exists(folderPath);
if (!folderExists) {
    await app.vault.createFolder(folderPath);
}

const newFileName = title + "_" + topic + "_" + d.getHours() + "-" + d.getMinutes() + ".md";
await tp.file.rename(newFileName);

await tp.file.move(folderPath + "/" + newFileName);
_%>
