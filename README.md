[woorim_dashboard.html](https://github.com/user-attachments/files/29026859/woorim_dashboard.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>우리팀 업무 대시보드</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Apple SD Gothic Neo', 'Malgun Gothic', sans-serif; background: #F5F4F8; color: #1a1a1a; min-height: 100vh; }

  header { background: #fff; border-bottom: 1px solid #E8E6F0; padding: 0 2rem; display: flex; align-items: center; height: 60px; gap: 12px; }
  header .logo { font-size: 18px; font-weight: 700; color: #534AB7; }
  header .sub { font-size: 13px; color: #888; margin-left: 4px; }

  .tabs { display: flex; gap: 0; background: #fff; border-bottom: 1px solid #E8E6F0; padding: 0 2rem; }
  .tab { padding: 14px 22px; font-size: 14px; color: #888; cursor: pointer; border-bottom: 2px solid transparent; background: none; border-top: none; border-left: none; border-right: none; font-family: inherit; }
  .tab:hover { color: #534AB7; }
  .tab.active { color: #534AB7; border-bottom-color: #534AB7; font-weight: 600; }

  .container { max-width: 1000px; margin: 0 auto; padding: 2rem 1.5rem; }

  .section { display: none; }
  .section.active { display: block; }

  .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; margin-bottom: 1.5rem; }
  .stat { background: #fff; border-radius: 12px; padding: 1.2rem 1.5rem; border: 1px solid #E8E6F0; }
  .stat-label { font-size: 12px; color: #999; margin-bottom: 8px; }
  .stat-value { font-size: 28px; font-weight: 700; }

  .card { background: #fff; border-radius: 14px; padding: 1.5rem; border: 1px solid #E8E6F0; margin-bottom: 16px; }
  .card-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.2rem; }
  .card-title { font-size: 15px; font-weight: 600; color: #1a1a1a; }

  .member-select { display: flex; gap: 8px; flex-wrap: wrap; }
  .member-btn { padding: 7px 18px; border-radius: 20px; border: 1.5px solid #E8E6F0; background: #fff; cursor: pointer; font-size: 13px; color: #555; font-family: inherit; transition: all 0.15s; }
  .member-btn:hover { border-color: #AFA9EC; color: #534AB7; }
  .member-btn.active { background: #EEEDFE; color: #3C3489; border-color: #AFA9EC; font-weight: 600; }

  .todo-list { list-style: none; }
  .todo-item { display: flex; align-items: center; gap: 12px; padding: 12px 0; border-bottom: 1px solid #F0EEF8; }
  .todo-item:last-child { border-bottom: none; }
  .todo-check { width: 20px; height: 20px; border-radius: 6px; border: 2px solid #D0CCEE; cursor: pointer; display: flex; align-items: center; justify-content: center; flex-shrink: 0; background: #fff; transition: all 0.15s; }
  .todo-check.done { background: #1D9E75; border-color: #1D9E75; }
  .todo-check.done::after { content: '✓'; color: #fff; font-size: 12px; font-weight: 700; }
  .todo-text { font-size: 14px; color: #1a1a1a; flex: 1; }
  .todo-text.done { text-decoration: line-through; color: #bbb; }
  .priority { font-size: 11px; padding: 3px 10px; border-radius: 10px; font-weight: 600; }
  .priority.high { background: #FAECE7; color: #993C1D; }
  .priority.mid { background: #FAEEDA; color: #854F0B; }
  .priority.low { background: #EAF3DE; color: #3B6D11; }
  .del-btn { background: none; border: none; cursor: pointer; color: #ccc; font-size: 16px; padding: 0 4px; transition: color 0.15s; }
  .del-btn:hover { color: #E24B4A; }

  .add-row { display: flex; gap: 8px; margin-top: 14px; }
  .add-row input, .add-row select { flex: 1; padding: 9px 14px; border-radius: 10px; border: 1.5px solid #E8E6F0; font-size: 13px; background: #FAFAFA; color: #1a1a1a; font-family: inherit; outline: none; }
  .add-row input:focus, .add-row select:focus { border-color: #AFA9EC; background: #fff; }
  .add-row select { flex: 0 0 90px; }
  .add-btn { padding: 9px 18px; border-radius: 10px; border: 1.5px solid #AFA9EC; background: #EEEDFE; color: #3C3489; cursor: pointer; font-size: 13px; font-weight: 600; font-family: inherit; white-space: nowrap; transition: background 0.15s; }
  .add-btn:hover { background: #CECBF6; }

  .kanban { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; }
  .kanban-col { background: #F5F4F8; border-radius: 12px; padding: 14px; }
  .kanban-col-title { font-size: 13px; font-weight: 600; color: #666; margin-bottom: 12px; display: flex; align-items: center; gap: 8px; }
  .kanban-badge { font-size: 11px; background: #fff; border: 1px solid #E8E6F0; border-radius: 10px; padding: 1px 8px; color: #999; }
  .task-card { background: #fff; border: 1px solid #E8E6F0; border-radius: 10px; padding: 12px 14px; margin-bottom: 8px; }
  .task-name { font-size: 13px; color: #1a1a1a; margin-bottom: 8px; font-weight: 500; }
  .task-meta { display: flex; align-items: center; gap: 8px; }
  .avatar { width: 22px; height: 22px; border-radius: 50%; display: inline-flex; align-items: center; justify-content: center; font-size: 10px; font-weight: 700; }
  .av-purple { background: #EEEDFE; color: #3C3489; }
  .av-teal { background: #E1F5EE; color: #085041; }
  .av-coral { background: #FAECE7; color: #993C1D; }
  .av-blue { background: #E6F1FB; color: #0C447C; }
  .task-person { font-size: 11px; color: #888; }
  .task-due { font-size: 11px; color: #bbb; margin-left: auto; }
  .add-task-btn { width: 100%; padding: 9px; border: 1.5px dashed #D0CCEE; border-radius: 10px; background: transparent; cursor: pointer; font-size: 12px; color: #aaa; font-family: inherit; margin-top: 4px; transition: all 0.15s; }
  .add-task-btn:hover { background: #fff; color: #534AB7; border-color: #AFA9EC; }

  .cal-nav { display: flex; align-items: center; gap: 10px; }
  .cal-nav-btn { padding: 6px 12px; border-radius: 8px; border: 1px solid #E8E6F0; background: #fff; cursor: pointer; font-size: 14px; color: #534AB7; font-family: inherit; }
  .cal-nav-btn:hover { background: #EEEDFE; }
  .cal-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 3px; margin-top: 12px; }
  .cal-day-label { text-align: center; font-size: 12px; color: #aaa; padding: 6px 0; font-weight: 600; }
  .cal-cell { min-height: 72px; border-radius: 10px; padding: 6px; border: 1px solid #F0EEF8; background: #fff; }
  .cal-cell.other-month { background: #FAFAFA; opacity: 0.5; }
  .cal-cell.today { border-color: #7F77DD; border-width: 2px; }
  .cal-num { font-size: 12px; color: #999; margin-bottom: 3px; }
  .cal-num.today-num { color: #534AB7; font-weight: 700; }
  .cal-event { font-size: 10px; padding: 2px 6px; border-radius: 5px; margin-bottom: 2px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .ev-purple { background: #EEEDFE; color: #3C3489; }
  .ev-teal { background: #E1F5EE; color: #085041; }
  .ev-coral { background: #FAECE7; color: #993C1D; }

  .notice-list { list-style: none; }
  .notice-item { padding: 14px 0; border-bottom: 1px solid #F0EEF8; display: flex; gap: 14px; align-items: flex-start; }
  .notice-item:last-child { border-bottom: none; }
  .notice-dot { width: 8px; height: 8px; border-radius: 50%; background: #7F77DD; margin-top: 6px; flex-shrink: 0; }
  .notice-title { font-size: 14px; color: #1a1a1a; font-weight: 500; margin-bottom: 4px; }
  .notice-date { font-size: 12px; color: #bbb; }
  .notice-content-text { font-size: 13px; color: #666; margin-top: 4px; }

  .btn-outline { padding: 7px 16px; border-radius: 10px; border: 1.5px solid #AFA9EC; background: #fff; color: #534AB7; cursor: pointer; font-size: 13px; font-weight: 600; font-family: inherit; transition: background 0.15s; }
  .btn-outline:hover { background: #EEEDFE; }

  /* 모달 */
  .modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.35); display: none; align-items: center; justify-content: center; z-index: 100; }
  .modal-overlay.open { display: flex; }
  .modal { background: #fff; border-radius: 16px; padding: 2rem; width: 360px; box-shadow: 0 8px 40px rgba(83,74,183,0.12); }
  .modal-title { font-size: 17px; font-weight: 700; margin-bottom: 1.2rem; color: #1a1a1a; }
  .modal label { font-size: 13px; color: #666; display: block; margin-bottom: 5px; margin-top: 12px; }
  .modal input, .modal select, .modal textarea { width: 100%; padding: 9px 14px; border-radius: 10px; border: 1.5px solid #E8E6F0; font-size: 13px; background: #FAFAFA; color: #1a1a1a; font-family: inherit; outline: none; }
  .modal input:focus, .modal select:focus, .modal textarea:focus { border-color: #AFA9EC; background: #fff; }
  .modal textarea { height: 80px; resize: none; }
  .modal-btns { display: flex; gap: 10px; margin-top: 1.5rem; justify-content: flex-end; }
  .btn-cancel { padding: 9px 18px; border-radius: 10px; border: 1.5px solid #E8E6F0; background: #fff; cursor: pointer; font-size: 13px; color: #888; font-family: inherit; }
  .btn-save { padding: 9px 18px; border-radius: 10px; border: none; background: #534AB7; color: #fff; cursor: pointer; font-size: 13px; font-weight: 700; font-family: inherit; transition: background 0.15s; }
  .btn-save:hover { background: #3C3489; }

  .empty { padding: 20px 0; font-size: 14px; color: #bbb; text-align: center; }

  @media (max-width: 600px) {
    .kanban { grid-template-columns: 1fr; }
    .stats { grid-template-columns: repeat(3, 1fr); }
    .tab { padding: 12px 14px; font-size: 12px; }
    .container { padding: 1rem; }
  }
</style>
</head>
<body>

<header>
  <span class="logo">🌿 우리팀</span>
  <span class="sub">업무 관리 대시보드</span>
</header>

<div class="tabs">
  <button class="tab active" onclick="showSection('todo')">✅ 내 할 일</button>
  <button class="tab" onclick="showSection('board')">📋 업무 현황판</button>
  <button class="tab" onclick="showSection('calendar')">📅 팀 일정</button>
  <button class="tab" onclick="showSection('notice')">📢 공지사항</button>
</div>

<div class="container">

  <!-- 내 할 일 -->
  <div class="section active" id="section-todo">
    <div class="stats" id="todo-stats"></div>
    <div class="card">
      <div class="card-header"><span class="card-title">👤 팀원 선택</span></div>
      <div class="member-select" id="member-select"></div>
    </div>
    <div class="card">
      <div class="card-header">
        <span class="card-title" id="todo-member-title">할 일 목록</span>
      </div>
      <ul class="todo-list" id="todo-list"></ul>
      <div class="add-row">
        <input type="text" id="new-todo" placeholder="새 할 일을 입력하세요..." />
        <select id="new-priority">
          <option value="high">🔴 높음</option>
          <option value="mid" selected>🟡 보통</option>
          <option value="low">🟢 낮음</option>
        </select>
        <button class="add-btn" onclick="addTodo()">+ 추가</button>
      </div>
    </div>
  </div>

  <!-- 업무 현황판 -->
  <div class="section" id="section-board">
    <div class="stats" id="board-stats"></div>
    <div class="card">
      <div class="card-header"><span class="card-title">📋 업무 진행 현황</span></div>
      <div class="kanban" id="kanban-board"></div>
    </div>
  </div>

  <!-- 팀 일정 -->
  <div class="section" id="section-calendar">
    <div class="card">
      <div class="card-header">
        <span class="card-title" id="cal-title"></span>
        <div class="cal-nav">
          <button class="cal-nav-btn" onclick="changeMonth(-1)">◀</button>
          <button class="cal-nav-btn" onclick="changeMonth(1)">▶</button>
          <button class="btn-outline" onclick="openEventModal()">+ 일정 추가</button>
        </div>
      </div>
      <div class="cal-grid" id="cal-grid"></div>
    </div>
  </div>

  <!-- 공지사항 -->
  <div class="section" id="section-notice">
    <div class="card">
      <div class="card-header">
        <span class="card-title">📢 공지사항</span>
        <button class="btn-outline" onclick="openNoticeModal()">+ 공지 작성</button>
      </div>
      <ul class="notice-list" id="notice-list"></ul>
    </div>
  </div>

</div>

<!-- 업무 추가 모달 -->
<div class="modal-overlay" id="modal-task">
  <div class="modal">
    <p class="modal-title">📋 업무 추가</p>
    <label>업무 이름</label>
    <input type="text" id="task-name" placeholder="업무 내용을 입력하세요" />
    <label>담당자</label>
    <select id="task-member"></select>
    <label>상태</label>
    <select id="task-status">
      <option value="0">할 일</option>
      <option value="1">진행 중</option>
      <option value="2">완료</option>
    </select>
    <label>마감일</label>
    <input type="date" id="task-due" />
    <div class="modal-btns">
      <button class="btn-cancel" onclick="closeModal('modal-task')">취소</button>
      <button class="btn-save" onclick="saveTask()">저장</button>
    </div>
  </div>
</div>

<!-- 일정 추가 모달 -->
<div class="modal-overlay" id="modal-event">
  <div class="modal">
    <p class="modal-title">📅 일정 추가</p>
    <label>일정 이름</label>
    <input type="text" id="event-name" placeholder="일정 내용을 입력하세요" />
    <label>날짜</label>
    <input type="date" id="event-date" />
    <label>종류</label>
    <select id="event-type">
      <option value="ev-purple">팀 미팅</option>
      <option value="ev-teal">업무</option>
      <option value="ev-coral">기타</option>
    </select>
    <div class="modal-btns">
      <button class="btn-cancel" onclick="closeModal('modal-event')">취소</button>
      <button class="btn-save" onclick="saveEvent()">저장</button>
    </div>
  </div>
</div>

<!-- 공지 작성 모달 -->
<div class="modal-overlay" id="modal-notice">
  <div class="modal">
    <p class="modal-title">📢 공지 작성</p>
    <label>제목</label>
    <input type="text" id="notice-title-input" placeholder="공지 제목을 입력하세요" />
    <label>내용</label>
    <textarea id="notice-content" placeholder="공지 내용을 입력하세요"></textarea>
    <div class="modal-btns">
      <button class="btn-cancel" onclick="closeModal('modal-notice')">취소</button>
      <button class="btn-save" onclick="saveNotice()">저장</button>
    </div>
  </div>
</div>

<script>
const MEMBERS = [
  { id: 'a', name: '강승현', av: 'av-purple', initials: '강' },
  { id: 'b', name: '김수정', av: 'av-teal', initials: '김' },
  { id: 'c', name: '신지혜', av: 'av-coral', initials: '신' },
  { id: 'd', name: '박혜윤', av: 'av-blue', initials: '박' },
];

let selectedMember = 'a';
let todos = {
  a: [{ text: '주간 보고서 작성', done: false, priority: 'high' }],
  b: [{ text: '기획안 초안 작성', done: false, priority: 'high' }],
  c: [{ text: '자료 조사', done: false, priority: 'mid' }],
  d: [{ text: '견적서 검토', done: false, priority: 'mid' }],
};
let tasks = [
  { name: '월간 리포트 작성', member: 'a', status: 1, due: '06/20' },
  { name: '신규 기획안 검토', member: 'b', status: 0, due: '06/22' },
  { name: '클라이언트 미팅 준비', member: 'a', status: 2, due: '06/18' },
  { name: '데이터 정리', member: 'c', status: 1, due: '06/25' },
  { name: '예산안 작성', member: 'd', status: 0, due: '06/28' },
];
let events = [
  { name: '주간 팀 미팅', date: '2025-06-17', type: 'ev-purple' },
  { name: '기획 발표', date: '2025-06-20', type: 'ev-teal' },
  { name: '월간 보고', date: '2025-06-26', type: 'ev-coral' },
];
let notices = [
  { title: '6월 팀 워크숍 일정 안내', content: '자세한 내용은 추후 공유드리겠습니다.', date: '2025.06.10' },
  { title: '연차 신청 마감 공지', content: '6월 30일까지 연차 신청 부탁드립니다.', date: '2025.06.08' },
];

let calYear = new Date().getFullYear();
let calMonth = new Date().getMonth();
const today = new Date();

function showSection(id) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.getElementById('section-' + id).classList.add('active');
  const idx = ['todo','board','calendar','notice'].indexOf(id);
  document.querySelectorAll('.tab')[idx].classList.add('active');
  if (id === 'todo') renderTodo();
  if (id === 'board') renderBoard();
  if (id === 'calendar') renderCalendar();
  if (id === 'notice') renderNotices();
}

function renderTodo() {
  document.getElementById('member-select').innerHTML = MEMBERS.map(m =>
    `<button class="member-btn ${m.id === selectedMember ? 'active' : ''}" onclick="selectMember('${m.id}')">${m.name}</button>`
  ).join('');
  const m = MEMBERS.find(x => x.id === selectedMember);
  document.getElementById('todo-member-title').textContent = m.name + '의 할 일';
  const list = todos[selectedMember] || [];
  const done = list.filter(t => t.done).length;
  document.getElementById('todo-stats').innerHTML = `
    <div class="stat"><div class="stat-label">전체 할 일</div><div class="stat-value">${list.length}</div></div>
    <div class="stat"><div class="stat-label">완료</div><div class="stat-value" style="color:#1D9E75">${done}</div></div>
    <div class="stat"><div class="stat-label">남은 일</div><div class="stat-value" style="color:#D85A30">${list.length - done}</div></div>
  `;
  document.getElementById('todo-list').innerHTML = list.length === 0
    ? '<div class="empty">할 일이 없어요! 아래에서 추가해 보세요 😊</div>'
    : list.map((t, i) => `
      <li class="todo-item">
        <div class="todo-check ${t.done ? 'done' : ''}" onclick="toggleTodo(${i})"></div>
        <span class="todo-text ${t.done ? 'done' : ''}">${t.text}</span>
        <span class="priority ${t.priority}">${t.priority === 'high' ? '높음' : t.priority === 'mid' ? '보통' : '낮음'}</span>
        <button class="del-btn" onclick="deleteTodo(${i})">🗑</button>
      </li>`).join('');
}

function selectMember(id) { selectedMember = id; renderTodo(); }
function toggleTodo(i) { todos[selectedMember][i].done = !todos[selectedMember][i].done; renderTodo(); }
function deleteTodo(i) { if (confirm('삭제할까요?')) { todos[selectedMember].splice(i, 1); renderTodo(); } }
function addTodo() {
  const txt = document.getElementById('new-todo').value.trim();
  if (!txt) return alert('할 일을 입력해 주세요!');
  todos[selectedMember].push({ text: txt, done: false, priority: document.getElementById('new-priority').value });
  document.getElementById('new-todo').value = '';
  renderTodo();
}

function renderBoard() {
  const cols = ['할 일', '진행 중 🔥', '완료 ✅'];
  const grouped = [0,1,2].map(s => tasks.filter(t => t.status === s));
  const total = tasks.length, done = grouped[2].length;
  document.getElementById('board-stats').innerHTML = `
    <div class="stat"><div class="stat-label">전체 업무</div><div class="stat-value">${total}</div></div>
    <div class="stat"><div class="stat-label">진행 중</div><div class="stat-value" style="color:#BA7517">${grouped[1].length}</div></div>
    <div class="stat"><div class="stat-label">완료</div><div class="stat-value" style="color:#1D9E75">${done}</div></div>
  `;
  document.getElementById('kanban-board').innerHTML = cols.map((col, ci) => `
    <div class="kanban-col">
      <div class="kanban-col-title">${col} <span class="kanban-badge">${grouped[ci].length}</span></div>
      ${grouped[ci].map(t => {
        const mem = MEMBERS.find(x => x.id === t.member);
        return `<div class="task-card">
          <div class="task-name">${t.name}</div>
          <div class="task-meta">
            <div class="avatar ${mem ? mem.av : 'av-purple'}">${mem ? mem.initials : '?'}</div>
            <span class="task-person">${mem ? mem.name : ''}</span>
            <span class="task-due">~${t.due}</span>
          </div>
        </div>`;
      }).join('')}
      <button class="add-task-btn" onclick="openTaskModal(${ci})">+ 업무 추가</button>
    </div>`).join('');
}

function openTaskModal(status) {
  document.getElementById('task-name').value = '';
  document.getElementById('task-due').value = '';
  document.getElementById('task-status').value = status;
  document.getElementById('task-member').innerHTML = MEMBERS.map(m => `<option value="${m.id}">${m.name}</option>`).join('');
  document.getElementById('modal-task').classList.add('open');
}
function saveTask() {
  const name = document.getElementById('task-name').value.trim();
  if (!name) return alert('업무 이름을 입력해 주세요!');
  const due = document.getElementById('task-due').value;
  const dueStr = due ? due.slice(5).replace('-', '/') : '미정';
  tasks.push({ name, member: document.getElementById('task-member').value, status: parseInt(document.getElementById('task-status').value), due: dueStr });
  closeModal('modal-task'); renderBoard();
}

function renderCalendar() {
  const months = ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'];
  document.getElementById('cal-title').textContent = `📅 ${calYear}년 ${months[calMonth]}`;
  const days = ['일','월','화','수','목','금','토'];
  const first = new Date(calYear, calMonth, 1).getDay();
  const last = new Date(calYear, calMonth + 1, 0).getDate();
  const prevLast = new Date(calYear, calMonth, 0).getDate();
  let html = days.map(d => `<div class="cal-day-label">${d}</div>`).join('');
  let cells = [];
  for (let i = first - 1; i >= 0; i--) cells.push({ day: prevLast - i, cur: false });
  for (let d = 1; d <= last; d++) cells.push({ day: d, cur: true });
  while (cells.length < 42) cells.push({ day: cells.length - last - first + 1, cur: false });
  cells.forEach(c => {
    const dateStr = `${calYear}-${String(calMonth+1).padStart(2,'0')}-${String(c.day).padStart(2,'0')}`;
    const isToday = c.cur && today.getFullYear() === calYear && today.getMonth() === calMonth && today.getDate() === c.day;
    const dayEvents = events.filter(e => e.date === dateStr);
    html += `<div class="cal-cell ${c.cur ? '' : 'other-month'} ${isToday ? 'today' : ''}">
      <div class="cal-num ${isToday ? 'today-num' : ''}">${c.day}</div>
      ${dayEvents.map(e => `<div class="cal-event ${e.type}">${e.name}</div>`).join('')}
    </div>`;
  });
  document.getElementById('cal-grid').innerHTML = html;
}

function changeMonth(d) {
  calMonth += d;
  if (calMonth > 11) { calMonth = 0; calYear++; }
  if (calMonth < 0) { calMonth = 11; calYear--; }
  renderCalendar();
}
function openEventModal() {
  document.getElementById('event-name').value = '';
  document.getElementById('event-date').value = '';
  document.getElementById('modal-event').classList.add('open');
}
function saveEvent() {
  const name = document.getElementById('event-name').value.trim();
  const date = document.getElementById('event-date').value;
  if (!name || !date) return alert('일정 이름과 날짜를 입력해 주세요!');
  events.push({ name, date, type: document.getElementById('event-type').value });
  closeModal('modal-event'); renderCalendar();
}

function renderNotices() {
  document.getElementById('notice-list').innerHTML = notices.length === 0
    ? '<div class="empty">공지사항이 없습니다.</div>'
    : notices.map(n => `<li class="notice-item">
        <div class="notice-dot"></div>
        <div>
          <div class="notice-title">${n.title}</div>
          ${n.content ? `<div class="notice-content-text">${n.content}</div>` : ''}
          <div class="notice-date">${n.date}</div>
        </div>
      </li>`).join('');
}
function openNoticeModal() {
  document.getElementById('notice-title-input').value = '';
  document.getElementById('notice-content').value = '';
  document.getElementById('modal-notice').classList.add('open');
}
function saveNotice() {
  const title = document.getElementById('notice-title-input').value.trim();
  if (!title) return alert('제목을 입력해 주세요!');
  const now = new Date();
  const date = `${now.getFullYear()}.${String(now.getMonth()+1).padStart(2,'0')}.${String(now.getDate()).padStart(2,'0')}`;
  notices.unshift({ title, content: document.getElementById('notice-content').value, date });
  closeModal('modal-notice'); renderNotices();
}

function closeModal(id) { document.getElementById(id).classList.remove('open'); }

// 초기 렌더링
renderTodo();
</script>
</body>
</html>
