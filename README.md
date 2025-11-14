
# Ex03 To-Do List using JavaScript
## Date:14.11.2025

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
## Todo.html
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Ex.03 — Todo Application (JavaScript)</title>
  <style>
    /* Simple, modern styling */
    :root{--bg:#0f1724;--card:#0b1220;--muted:#94a3b8;--accent:#7c3aed;--glass:rgba(255,255,255,0.03)}
    *{box-sizing:border-box}
    body{font-family:Inter,system-ui,Segoe UI,Roboto,Helvetica,Arial,sans-serif;margin:0;background:linear-gradient(180deg,#081226 0%,#071228 100%);color:#e6eef8;min-height:100vh;display:flex;align-items:center;justify-content:center;padding:32px}
    .wrap{width:100%;max-width:920px}
    header{display:flex;align-items:center;justify-content:space-between;margin-bottom:18px}
    header h1{margin:0;font-size:20px}
    .card{background:var(--card);border-radius:12px;padding:18px;box-shadow:0 6px 30px rgba(2,6,23,0.6)}

    .top{display:flex;gap:12px;align-items:center;margin-bottom:14px}
    .top input[type=text]{flex:1;padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:var(--glass);color:inherit}
    .top button{padding:10px 14px;border-radius:10px;border:0;background:var(--accent);color:white;cursor:pointer}

    .controls{display:flex;flex-wrap:wrap;gap:10px;margin:12px 0}
    .controls .filters{display:flex;gap:8px}
    .controls button{background:transparent;border:1px solid rgba(255,255,255,0.04);padding:8px 10px;border-radius:8px;color:var(--muted);cursor:pointer}
    .controls button.active{border-color:var(--accent);color:white}

    ul#list{list-style:none;margin:0;padding:6px;min-height:180px}
    li.task{display:flex;gap:12px;align-items:center;padding:10px;border-radius:8px;margin-bottom:8px;background:linear-gradient(90deg, rgba(255,255,255,0.02), transparent);border:1px solid rgba(255,255,255,0.02)}
    li.task.dragging{opacity:0.6}
    .task .left{display:flex;gap:10px;align-items:center;flex:1}
    .task input[type=checkbox]{width:18px;height:18px}
    .task .meta{display:flex;flex-direction:column}
    .task .meta .title{font-weight:600}
    .task .meta .subtitle{font-size:12px;color:var(--muted)}
    .task .actions{display:flex;gap:8px}
    .actions button{background:transparent;border:0;color:var(--muted);cursor:pointer}
    .edit-mode{border:1px dashed rgba(255,255,255,0.06);padding:8px;border-radius:8px}

    footer{margin-top:16px;color:var(--muted);font-size:13px;text-align:center}
    .footer-small{font-size:12px;color:var(--muted)}

    /* responsive */
    @media (max-width:640px){body{padding:12px}.top{flex-direction:column}.top input[type=text]{width:100%}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Ex.03 — Todo Application (vanilla JavaScript)</h1>
      <div class="small">Opened: Sat, 15 Mar 2025 • Due: Sat, 1 Nov 2025</div>
    </header>

    <section class="card">
      <div class="top">
        <input id="newTaskInput" type="text" placeholder="Add a new task — press Enter or click Add" aria-label="New task">
        <button id="addBtn">Add</button>
      </div>

      <div class="controls">
        <div class="filters">
          <button data-filter="all" class="active">All</button>
          <button data-filter="active">Active</button>
          <button data-filter="completed">Completed</button>
        </div>

        <input id="search" type="text" placeholder="Search tasks..." style="margin-left:auto;padding:8px;border-radius:8px;background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted)" />

        <button id="clearCompleted">Clear completed</button>
        <button id="exportBtn">Export JSON</button>
        <button id="importBtn">Import JSON</button>
        <input id="fileImport" type="file" accept="application/json" style="display:none" />
      </div>

      <ul id="list" aria-live="polite"></ul>

      <div style="margin-top:12px;display:flex;justify-content:space-between;align-items:center;gap:14px;flex-wrap:wrap">
        <div id="count" class="footer-small">0 tasks</div>
        <div style="font-size:13px;color:var(--muted)">Tip: drag to reorder tasks</div>
      </div>
    </section>

    <footer>
      <!-- Replace with your actual name and register number before submission -->
      <div>Submitted by: <strong>Your Name</strong> — Register Number: <strong>XXXXXXXX</strong></div>
      <div class="footer-small">Follow submission steps: fork the provided repo, clone, replace this HTML, run locally, take a full screenshot of output, and include your GitHub repo URL in the online text.</div>
    </footer>
  </div>

  <script>
    // Single-file Todo app with full-featured behavior + persistence + drag-and-drop + import/export
    const newTaskInput = document.getElementById('newTaskInput');
    const addBtn = document.getElementById('addBtn');
    const listEl = document.getElementById('list');
    const filters = document.querySelectorAll('[data-filter]');
    const countEl = document.getElementById('count');
    const searchInput = document.getElementById('search');
    const clearCompletedBtn = document.getElementById('clearCompleted');
    const exportBtn = document.getElementById('exportBtn');
    const importBtn = document.getElementById('importBtn');
    const fileImport = document.getElementById('fileImport');

    let tasks = []; // {id, text, completed, createdAt}
    let filter = 'all';
    let dragSrcEl = null;

    // Persistence
    function save(){localStorage.setItem('ex03_tasks_v1', JSON.stringify(tasks));}
    function load(){try{tasks = JSON.parse(localStorage.getItem('ex03_tasks_v1')||'[]')}catch(e){tasks=[]}render();}

    function uid(){return Date.now().toString(36)+Math.random().toString(36).slice(2,7)}

    function addTask(text){text = text && text.trim(); if(!text) return; const t={id:uid(),text,completed:false,createdAt:Date.now()}; tasks.unshift(t); save(); render();}

    function editTask(id,newText){const t = tasks.find(x=>x.id===id); if(!t) return; t.text = newText; save(); render();}
    function toggleTask(id){const t = tasks.find(x=>x.id===id); if(!t) return; t.completed = !t.completed; save(); render();}
    function deleteTask(id){tasks = tasks.filter(x=>x.id!==id); save(); render();}
    function clearCompleted(){tasks = tasks.filter(t=>!t.completed); save(); render();}

    function setFilter(f){filter=f; filters.forEach(b=>b.classList.toggle('active', b.dataset.filter===f)); render();}

    function matchesFilter(t){if(filter==='all') return true; if(filter==='active') return !t.completed; return t.completed}

    function render(){
      listEl.innerHTML='';
      const q = (searchInput.value||'').toLowerCase();
      const shown = tasks.filter(t=>matchesFilter(t) && t.text.toLowerCase().includes(q));
      shown.forEach(task=>{
        const li = document.createElement('li'); li.className='task'; li.draggable = true; li.dataset.id = task.id;

        // DnD handlers
        li.addEventListener('dragstart', e=>{dragSrcEl = li; li.classList.add('dragging'); e.dataTransfer.effectAllowed='move';});
        li.addEventListener('dragend', ()=>{li.classList.remove('dragging'); dragSrcEl=null});
        li.addEventListener('dragover', e=>{e.preventDefault(); e.dataTransfer.dropEffect='move'});
        li.addEventListener('drop', e=>{e.preventDefault(); if(!dragSrcEl || dragSrcEl===li) return; handleDrop(dragSrcEl, li)});

        const left = document.createElement('div'); left.className='left';
        const cb = document.createElement('input'); cb.type='checkbox'; cb.checked = !!task.completed; cb.addEventListener('change', ()=>toggleTask(task.id));
        const meta = document.createElement('div'); meta.className='meta';
        const title = document.createElement('div'); title.className='title'; title.textContent = task.text; if(task.completed) title.style.textDecoration='line-through';
        const subtitle = document.createElement('div'); subtitle.className='subtitle'; subtitle.textContent = 'Added: '+new Date(task.createdAt).toLocaleString();
        meta.appendChild(title); meta.appendChild(subtitle);
        left.appendChild(cb); left.appendChild(meta);

        const actions = document.createElement('div'); actions.className='actions';
        const editBtn = document.createElement('button'); editBtn.title='Edit'; editBtn.innerHTML='✏️'; editBtn.addEventListener('click', ()=>enterEditMode(li, task));
        const delBtn = document.createElement('button'); delBtn.title='Delete'; delBtn.innerHTML='🗑️'; delBtn.addEventListener('click', ()=>{ if(confirm('Delete this task?')) deleteTask(task.id); });
        actions.appendChild(editBtn); actions.appendChild(delBtn);

        li.appendChild(left); li.appendChild(actions);
        listEl.appendChild(li);
      });

      countEl.textContent = tasks.length + ' tasks';
    }

    function enterEditMode(li, task){
      li.innerHTML='';
      const inp = document.createElement('input'); inp.type='text'; inp.value = task.text; inp.className='edit-mode'; inp.style.flex='1';
      const saveBtn = document.createElement('button'); saveBtn.textContent='Save'; saveBtn.addEventListener('click', ()=>{ const v = inp.value.trim(); if(!v) return alert('Task cannot be empty'); editTask(task.id, v)});
      const cancelBtn = document.createElement('button'); cancelBtn.textContent='Cancel'; cancelBtn.addEventListener('click', render);
      const left = document.createElement('div'); left.style.display='flex'; left.style.gap='8px'; left.style.flex='1'; left.appendChild(inp);
      const right = document.createElement('div'); right.style.display='flex'; right.style.gap='8px'; right.appendChild(saveBtn); right.appendChild(cancelBtn);
      li.appendChild(left); li.appendChild(right);
      inp.focus(); inp.setSelectionRange(inp.value.length, inp.value.length);
    }

    function handleDrop(src, target){
      const idSrc = src.dataset.id; const idT = target.dataset.id; const iSrc = tasks.findIndex(t=>t.id===idSrc); const iT = tasks.findIndex(t=>t.id===idT);
      if(iSrc<0||iT<0) return; const [item] = tasks.splice(iSrc,1); tasks.splice(iT,0,item); save(); render();
    }

    // Add handlers
    addBtn.addEventListener('click', ()=>{ addTask(newTaskInput.value); newTaskInput.value=''; newTaskInput.focus(); });
    newTaskInput.addEventListener('keydown', e=>{ if(e.key==='Enter'){ addTask(newTaskInput.value); newTaskInput.value=''; }});
    filters.forEach(b=>b.addEventListener('click', ()=>setFilter(b.dataset.filter)));
    searchInput.addEventListener('input', render);
    clearCompletedBtn.addEventListener('click', ()=>{ if(confirm('Clear all completed tasks?')) clearCompleted(); });

    // Export / Import
    exportBtn.addEventListener('click', ()=>{
      const dataStr = JSON.stringify(tasks, null, 2);
      const blob = new Blob([dataStr], {type:'application/json'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a'); a.href = url; a.download = 'todo-export.json'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
    });
    importBtn.addEventListener('click', ()=>fileImport.click());
    fileImport.addEventListener('change', ()=>{
      const f = fileImport.files[0]; if(!f) return; const reader = new FileReader(); reader.onload = e=>{ try{ const imported = JSON.parse(e.target.result); if(!Array.isArray(imported)) throw new Error('Invalid file'); // simple merge
          imported.forEach(it=>{ if(!it.id) it.id = uid(); });
          tasks = imported.concat(tasks); save(); render(); alert('Import successful');
        }catch(err){alert('Import failed: '+err.message)}
      }; reader.readAsText(f);
    });

    // Keyboard shortcuts
    document.addEventListener('keydown', e=>{
      if((e.ctrlKey||e.metaKey) && e.key.toLowerCase()==='f'){ e.preventDefault(); searchInput.focus(); }
      if(e.key==='/' && document.activeElement!==searchInput){ e.preventDefault(); searchInput.focus(); }
    });

    // Initial load
    load();

    // For first-run demo tasks if none
    if(tasks.length===0){ addTask('Welcome! Edit or delete this task.'); addTask('Use the search box (/) and filters.'); }
  </script>
</body>
</html>

```
## OUTPUT

<img width="1919" height="1087" alt="image" src="https://github.com/user-attachments/assets/e535f791-c5f6-4015-8e67-f0b72c335c84" />



## RESULT
The program for creating To-do list using JavaScript is executed successfully.
