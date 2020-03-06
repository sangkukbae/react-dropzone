# react dropzone

## [react-dropzone](https://react-dropzone.js.org/)

* `getRootProps`: Returns the props you should apply to the root drop container you render
* `getInputProps`: Returns the props you should apply to hidden file input you render
* `acceptedFiles`: Accepted files

```jsx
import React from "react";
import { useDropzone } from "react-dropzone";

export default function Basic(props) {
  const { acceptedFiles, getRootProps, getInputProps } = useDropzone();

  const files = acceptedFiles.map(file => (
    <li key={file.path}>
      {file.path} - {file.size} bytes
    </li>
  ));

  return (
    <section className="container">
      <div {...getRootProps({ className: "dropzone" })}>
        <input {...getInputProps()} />
        <p>Drag 'n' drop some files here, or click to select files</p>
      </div>
      <aside>
        <h4>Files</h4>
        <ul>{files}</ul>
      </aside>
    </section>
  );
}
```

[https://codesandbox.io/s/react-dropzone-wzqkk](https://codesandbox.io/s/react-dropzone-wzqkk)

## [react-dropzone-uploader](https://react-dropzone-uploader.js.org/)

* `getUploadParams` is a callback that receives a `fileWithMeta` object and returns the params needed to upload the file.
* `onChangeStatus`  is called every time a file's status changes: `fileWithMeta.meta.status`.
* `onSubmit` is called when a user presses the submit button.
* `Accepted Files`: To control which files can be dropped or picked, you can use the accept prop, which is really the [HTML5 input accept attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#Limiting_accepted_file_types).

```jsx
import React from "react";
import "react-dropzone-uploader/dist/styles.css";
import Dropzone from "react-dropzone-uploader";

const MyUploader = () => {
  // specify upload params and url for your files
  const getUploadParams = ({ meta }) => {
    return { url: "https://httpbin.org/post" };
  };

  // called every time a file's `status` changes
  const handleChangeStatus = ({ meta, file }, status) => {
    console.log(status, meta, file);
  };

  // receives array of files that are done uploading when submit button is clicked
  const handleSubmit = (files, allFiles) => {
    console.log(files.map(f => f.meta));
    allFiles.forEach(f => f.remove());
  };

  return (
    <Dropzone
      getUploadParams={getUploadParams}
      onChangeStatus={handleChangeStatus}
      onSubmit={handleSubmit}
      accept="image/*,audio/*,video/*"
    />
  );
};

export default MyUploader;
```

[https://codesandbox.io/s/react-dropzone-uploader-g1c0d](https://codesandbox.io/s/react-dropzone-uploader-g1c0d)

## [HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)

* `ondragenter`: a dragged item enters a valid drop target.
* `ondragover`: a dragged item is being dragged over a valid drop target, every few hundred milliseconds.
* `ondrag`: a dragged item (element or text selection) is dragged.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
<div id="output" style="min-height: 200px; white-space: pre; border: 1px solid black;"
     ondragenter="document.getElementById('output').textContent = ''; event.stopPropagation(); event.preventDefault();"
     ondragover="event.stopPropagation(); event.preventDefault();"
     ondrop="event.stopPropagation(); event.preventDefault();
     dodrop(event);">
     DROP FILES HERE FROM FINDER OR EXPLORER
</div>
</body>
</html>
```

```javascript
function dodrop(event)
{
  var dt = event.dataTransfer;
  var files = dt.files;

  var count = files.length;
  output("File Count: " + count + "\n");

    for (var i = 0; i < files.length; i++) {
      output(" File " + i + ":\n(" + (typeof files[i]) + ") : <" + files[i] + " > " +
             files[i].name + " " + files[i].size + "\n");
    }
}

function output(text)
{
  document.getElementById("output").textContent += text;
  //dump(text);
}
```

[https://codepen.io/sangkukbae/pen/zYGPZbj](https://codepen.io/sangkukbae/pen/zYGPZbj)

**참고 자료**
* [https://medium.com/backticks-tildes/drag-and-drop-with-html5-368a1bb72881](https://medium.com/backticks-tildes/drag-and-drop-with-html5-368a1bb72881)
* [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#Limiting_accepted_file_types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#Limiting_accepted_file_types)

