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

if (!folderWithPrefixPath) { 
	return;
}

const title = await tp.system.prompt("File Name");
const topic = await tp.system.prompt("Topic");
const d = new Date();

const folderPath = folderWithPrefixPath + "/" + topic + "/" + d.getDate() + "-" + (d.getMonth() + 1) + "-" + d.getFullYear();

const folderExists = await app.vault.adapter.exists(folderPath);
if (!folderExists) {
    await app.vault.createFolder(folderPath);
}

const newFileName = title + "_" + topic + "_" + d.getHours() + "-" + d.getMinutes() + ".excalidraw";

const filePath = folderPath + "/" + newFileName;


await tp.file.create_new(tp.file.find_tfile("z_ressources/templates/lecture_note.excalidraw"), filePath);


_%>
