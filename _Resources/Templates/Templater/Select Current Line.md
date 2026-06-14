**Description** Selects all text on currrent line

<%*
	const editor = app.workspace.activeEditor.editor;
	// cm is codemirror object (what obsidian uses under the hood)
	const cm = editor.cm;
	
	const selectionRange = cm.viewState.state.selection.ranges[0];
	
	const cursor = editor.getCursor();
	const currLineContent = editor.getLine(cursor.line);
	const currLineLength = currLineContent.length;

	const cursorPositInLine = cursor.ch;

	const start = selectionRange.from - cursorPositInLine;
	const end = selectionRange.from + currLineLength - cursorPositInLine;
	cm.dispatch({selection: {anchor: start, head: end}})
	return null
%>



