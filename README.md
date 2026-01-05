<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Bible Journal</title>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, sans-serif;
      background: #f9f9f9;
      padding: 20px;
      max-width: 600px;
      margin: auto;
    }
    h1 {
      text-align: center;
    }
    label {
      font-weight: 600;
      margin-top: 15px;
      display: block;
    }
    input, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    textarea {
      min-height: 80px;
    }
    button {
      margin-top: 20px;
      width: 100%;
      padding: 12px;
      background: #007aff;
      color: white;
      border: none;
      border-radius: 10px;
      font-size: 18px;
    }
    .entry {
      background: white;
      padding: 15px;
      border-radius: 10px;
      margin-top: 20px;
      position: relative;
    }
    .entry small {
      color: #666;
    }
    .delete-btn {
      background: #ff3b30;
      color: white;
      border: none;
      border-radius: 6px;
      padding: 6px 10px;
      font-size: 14px;
      position: absolute;
      top: 15px;
      right: 15px;
    }
  </style>
</head>
<body>

<h1>📖 Bible Journal</h1>

<label>Date</label>
<input type="date" id="date" />

<label>What I Read</label>
<textarea id="read"></textarea>

<label>What I Learned</label>
<textarea id="learned"></textarea>

<label>How I Can Apply It</label>
<textarea id="apply"></textarea>

<button onclick="saveEntry()">Save Entry</button>

<div id="entries"></div>

<script>
  const entriesDiv = document.getElementById("entries");

  function saveEntry() {
    const entry = {
      date: date.value,
      read: read.value,
      learned: learned.value,
      apply: apply.value
    };

    const entries = JSON.parse(localStorage.getItem("journal")) || [];
    entries.unshift(entry);
    localStorage.setItem("journal", JSON.stringify(entries));

    read.value = learned.value = apply.value = "";
    loadEntries();
  }

  function deleteEntry(index) {
    if (!confirm("Delete this entry?")) return;

    const entries = JSON.parse(localStorage.getItem("journal")) || [];
    entries.splice(index, 1);
    localStorage.setItem("journal", JSON.stringify(entries));
    loadEntries();
  }

  function loadEntries() {
    const entries = JSON.parse(localStorage.getItem("journal")) || [];
    entriesDiv.innerHTML = "";

    entries.forEach((e, index) => {
      entriesDiv.innerHTML += `
        <div class="entry">
          <button class="delete-btn" onclick="deleteEntry(${index})">Delete</button>
          <small>${e.date || ""}</small>
          <p><strong>Read:</strong> ${e.read}</p>
          <p><strong>Learned:</strong> ${e.learned}</p>
          <p><strong>Apply:</strong> ${e.apply}</p>
        </div>
      `;
    });
  }

  loadEntries();
</script>

</body>
</html>


