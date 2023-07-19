# blog
#creation of a blog drag and drop a choosen file 
<!DOCTYPE html>
<html>
<head>
  <style>
    #drop-area {
      width: 300px;
      height: 200px;
      border: 500px dashed #ccc;
      border-radius: 5px;
      padding: 25px;
      text-align: center;
      font-family: times new roman, sans-serif;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div id="drop-area">
    Drag and drop files here<br/>
    or<br/>
    <input type="file" id="file-input" multiple />
    <label for="file-input">Browse Files</label>
  </div>

  <script>
    var dropArea = document.getElementById('drop-area');

    // Prevent default drag behaviors
    ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
      dropArea.addEventListener(eventName, preventDefaults, false);
      document.body.addEventListener(eventName, preventDefaults, false);
    });

    // Highlight drop area when item is dragged over it
    ['dragenter', 'dragover'].forEach(eventName => {
      dropArea.addEventListener(eventName, highlight, false);
    });

    // Unhighlight drop area when item is dragged out of it
    ['dragleave', 'drop'].forEach(eventName => {
      dropArea.addEventListener(eventName, unhighlight, false);
    });

    // Handle dropped files
    dropArea.addEventListener('drop', handleDrop, false);

    function preventDefaults(e) {
      e.preventDefault();
      e.stopPropagation();
    }

    function highlight() {
      dropArea.classList.add('highlight');
    }

    function unhighlight() {
      dropArea.classList.remove('highlight');
    }

    function handleDrop(e) {
      var dt = e.dataTransfer;
      var files = dt.files;

      handleFiles(files);
    }

    function handleFiles(files) {
      files = [...files];
      files.forEach(uploadFile);
    }

    function uploadFile(file) {
      var fileType = file.type;
      var validTypes = ['image/jpeg', 'image/png', 'image/gif', 'audio/mpeg', 'audio/ogg', 'video/mp4'];

      if (validTypes.indexOf(fileType) === -1) {
        console.log('Invalid file type: ' + fileType);
        return;
      }

      var reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onloadend = function () {
        var img = document.createElement('img');
        img.src = reader.result;
        document.body.appendChild(img);
      };
    }
  </script>
</body>
</html>
