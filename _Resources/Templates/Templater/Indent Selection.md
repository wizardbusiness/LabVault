<%*
// updating a selection deselects it and resets the cursor position. I want to reselect the new indented text, and the start position of the selection stays the same. 
const editor = app.workspace.activeEditor.editor
const cm = editor.cm
const selectionRange = cm.viewState.state.selection.ranges[0]
// the value of 'from' is an integer equal to the number of chars the selection start is from the beginning of the document, where 0 <= start <= doc end 
const selectionStart = selectionRange.from
const currentSelectionText = await tp.file.selection();
const lines = currentSelectionText.split(/\r?\n/);
const indent = "> "
const indentedLines = lines.map((line) => indent + line)
const indentedSelection = indentedLines.join("\n")

// the updated selection is longer, so we remeasure it from the start of the og selection. 
const selectionEnd = selectionStart + indentedSelection.length
// reselection needs to happen AFTER the selection is returned, hence the timeout wrapper
setTimeout(() => {
	cm.dispatch({selection: {anchor: selectionStart, head: selectionEnd}});
}, 0);

return indentedSelection
%> 
