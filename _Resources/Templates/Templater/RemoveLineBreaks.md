<%* 
const selection = await tp.file.selection()
const lines = selection.split(/\r?\n/)
const noBreaks = lines.filter((line) => {
	// matches lines with 1 or more words on them
	const lineWithContent = line.match(/\w+/)
	return lineWithContent
})
const updated = noBreaks.join("\n")
return updated
%>



