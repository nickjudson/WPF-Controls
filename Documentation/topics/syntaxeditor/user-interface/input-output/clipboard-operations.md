---
title: "Clipboard Operations"
page-title: "Clipboard Operations - SyntaxEditor Input/Output Features"
order: 3
---
# Clipboard Operations

SyntaxEditor makes use the Windows clipboard as a temporary repository for data for cut/copy/paste operations. SyntaxEditor places text on the clipboard using the `DataFormats.UnicodeText` and `DataFormats.Text` formats.

## Cutting and Copying Text

The [IEditorView](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView).[CutLineToClipboard](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView.CutLineToClipboard*) and [CopyToClipboard](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView.CopyToClipboard*) methods may be used to cut and copy text to the clipboard respectively.  The text that is placed on the clipboard is the currently selected text designated by the [IEditorView](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView).[Selection](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView.Selection).

If the selection length is zero, or in other words, no text is selected, a feature of SyntaxEditor is to place the entire line of text on the clipboard of which the caret is currently on.  You can control whether blank lines will overwrite the clipboard by using the [CanCutCopyBlankLineWhenNoSelection](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.CanCutCopyBlankLineWhenNoSelection) property.

There are [CutAndAppendToClipboard](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView.CutAndAppendToClipboard*) and [CopyAndAppendToClipboard](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView.CopyAndAppendToClipboard*) methods on [IEditorView](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView) as well, that append to the clipboard instead of replacing the contents of the clipboard.

This code demonstrates how to copy the currently selected text to the clipboard:

```csharp
editor.ActiveView.CopyToClipboard();
```

## Cutting and Copying with HTML and RTF Export

The text that is copied can also be copied to the clipboard with HTML and RTF exported highlighting.  Then, if you paste the text in an application such as Word, the text will appear highlighted.

The [SyntaxEditor](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor).[CanCutCopyDragWithHtml](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.CanCutCopyDragWithHtml) property controls whether HTML exporting is performed whenever cutting or copying to the clipboard.

The [SyntaxEditor](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor).[CanCutCopyDragWithRtf](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.CanCutCopyDragWithRtf) property controls whether RTF exporting is performed whenever cutting or copying to the clipboard.

## Pasting Text

The [IEditorView](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView).[PasteFromClipboard](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IEditorView.PasteFromClipboard*) method allows text on the clipboard to be pasted into the document, thereby replacing any text that is currently selected within the view from which the method is called.

If text was copied from a SyntaxEditor when the selection length was zero (see above) the entire line of text that was copied will be inserted above the line in which the caret is currently located. In this situation, the caret is not moved.

This code demonstrates how to paste text from the clipboard:

```csharp
editor.ActiveView.PasteFromClipboard();
```

## The Clipboard and XBAP Security

If an XBAP is deployed with Internet security restrictions in place, the XBAP is not permitted to access the Windows clipboard.  SyntaxEditor properly recognizes this scenario, and maintains an internal clipboard of its own so that text can be copied and pasted between SyntaxEditor instances or within the same SyntaxEditor control.

## Customizing Text to be Cut, Copied, or Dragged

Sometimes it is useful to be able to customize the text, or objects, to be cut, copied, or dragged.  The [SyntaxEditor](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor).[CutCopyDrag](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.CutCopyDrag) event that fires before text is cut or copied to the clipboard, and also before a drag occurs.

In its event arguments, it passes the [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) that is to be copied as well as the type of operation that will be performed.  When the event fires, the [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) has already been initialized with `DataFormats.UnicodeText` and `DataFormats.Text` entries based on the current selection in the editor.  The [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) can be modified to customize what is sent to the clipboard or dragged.

> [!NOTE]
> The [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) interface is used instead of `IDataObject` because `IDataObject` will throw a security exception in most XBAP applications. [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) is designed to use many of the same method signatures that `IDataObject` does.

## Tracking Clipboard Change Events

Since the [SyntaxEditor](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor).[CutCopyDrag](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.CutCopyDrag) event fires any time text is cut or copied from the control, it can also be used to maintain an external clipboard-setting history in your application.  This is useful for maintaining a clipboard ring for your application.

## Customizing Text to be Pasted or Dropped

Just like the [SyntaxEditor](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor).[CutCopyDrag](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.CutCopyDrag) event, an event is provided to allow for customization of text that is to be pasted or dropped onto the editor.  The [PasteDragDrop](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.SyntaxEditor.PasteDragDrop) event fires in several situations:

- Paste operations
- Paste completion
- CanPaste checks
- Drag enter
- Drag/drop

This event passes the [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) that is to be pasted/dropped and the source of the event.  The [IDataStore](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.IDataStore) is similar to `IDataObject` (see note above) and provides access to any clipboard data (such as non-text formats) that was used to trigger the event.

The [PasteDragDropEventArgs](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.PasteDragDropEventArgs).[Text](xref:ActiproSoftware.Windows.Controls.SyntaxEditor.PasteDragDropEventArgs.Text) property can be set to the text to be inserted.  It also can be set to `null` to insert nothing.

> [!NOTE]
> For CanPaste checks and drag enter scenarios, the event arg's `Text` property can be set to any non-null value to indicate that the object can be pasted or dropped.
