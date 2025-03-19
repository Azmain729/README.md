<!DOCTYPE html>
<html>
<head>
  <title>Notes App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 20px;
    }
    h1 {
      text-align: center;
    }
    textarea {
      width: 100%;
      height: 100px;
      padding: 10px;
      font-size: 16px;
      margin-bottom: 10px;
    }
    button {
      padding: 10px 15px;
      font-size: 16px;
      margin-right: 10px;
      cursor: pointer;
    }
    .note {
      background: #fff;
      padding: 10px;
      margin-top: 10px;
      border-left: 5px solid #007BFF;
    }
    .delete-btn {
      color: red;
      cursor: pointer;
      float: right;
    }
  </style>
</head>
<body>

  <h1>Notes App</h1>
  <textarea id="noteInput" placeholder="Write your note here..."></textarea>
  <br>
  <button onclick="addNote()">Save Note</button>
  <button onclick="clearNotes()">Clear All Notes</button>

  <div id="notesContainer"></div>

  <script>
    // Load notes from localStorage on page load
    window.onload = function() {
      loadNotes();
    };

    function addNote() {
      const noteText = document.getElementById("noteInput").value.trim();
      if (noteText === "") return alert("Please write something!");
      
      let notes = JSON.parse(localStorage.getItem("notes")) || [];
      notes.push(noteText);
      localStorage.setItem("notes", JSON.stringify(notes));
      document.getElementById("noteInput").value = "";
      loadNotes();
    }

    function loadNotes() {
      const notesContainer = document.getElementById("notesContainer");
      notesContainer.innerHTML = "";
      let notes = JSON.parse(localStorage.getItem("notes")) || [];

      notes.forEach((note, index) => {
        const noteDiv = document.createElement("div");
        noteDiv.className = "note";
        noteDiv.innerHTML = `
          ${note}
          <span class="delete-btn" onclick="deleteNote(${index})">Delete</span>
        `;
        notesContainer.appendChild(noteDiv);
      });
    }

    function deleteNote(index) {
      let notes = JSON.parse(localStorage.getItem("notes")) || [];
      notes.splice(index, 1);
      localStorage.setItem("notes", JSON.stringify(notes));
      loadNotes();
    }

    function clearNotes() {
      if (confirm("Are you sure you want to delete all notes?")) {
        localStorage.removeItem("notes");
        loadNotes();
      }
    }
  </script>

</body>
</html>
