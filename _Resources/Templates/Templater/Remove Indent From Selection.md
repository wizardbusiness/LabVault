<%*

	const editor = app.workspace.activeEditor.editor
	const cm = editor.cm
	let selectionRange = cm.viewState.state.selection.ranges[0]
	// the value of 'from' is an integer equal to the number of chars the selection start is from the beginning of the document, where 0 <= start <= doc end 
	let selectionStart = selectionRange.from

	const selection = await tp.file.selection()
	
	const lines = selection.split(/\r?\n/);
	// remove starting ">" followed by 0 or more whitespace chars pattern
	const updatedLines = lines.map((line) => line.replace(/^>\s*/, ""))
	const updatedSelection = updatedLines.join("\n")
	const updatedSelectionEnd = selectionStart + updatedSelection.length

	setTimeout(() => {
		cm.dispatch({selection: {anchor: selectionStart, head: updatedSelectionEnd}})
	}, 0)
	
	return updatedSelection
%>


