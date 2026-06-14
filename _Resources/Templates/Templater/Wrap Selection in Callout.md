<%* 
	let calloutType = await tp.system.suggester((item) => item, ["example", "bash", "ps", "py", "note", "seealso", "abstract", "summary", "tldr", "info", "tip", "hint", "important", "success", "question", "help", "faq", "warning", "caution", "attention", "failure", "fail", "missing", "danger", "error", "bug", "check", "done", "quote", "cite", "js",  "bug", "code", "detail", "decomp", "recomp"]); 

	const selectedText = await tp.file.selection();
	const lines = selectedText.split(/\r?\n/);
	const indentedLines = lines.map((line) => "> " + line);
	const indentedBlock = indentedLines.join("\n");	
	
	const topLine = lines[0];
	let calloutHeader = `[!${calloutType}]- ${calloutType}\n`;
	if (!topLine.startsWith(">")) {
		calloutHeader = "> " + calloutHeader;
	} else {
		const lineContentIndex = topLine.search(/[^> ]/);
		const indentPrefix = topLinePrefix.slice(0, lineContentIndex);
		const indentLevel = indentPrefix.match(/> /g).length;
		const addedIndent = "> ".repeat(indentLevel + 1);
		calloutHeader = addedIndent + calloutHeader;
	}
	
	tR = calloutHeader + indentedBlock;

	const editor = app.workspace.activeEditor.editor

	const from = editor.getCursor();
	
	return tR
%>


