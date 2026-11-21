<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HTML Box</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lz-string/1.5.0/lz-string.min.js"></script>
  <style>
    * { box-sizing: border-box; }
    body { margin: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; }
    textarea { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace; }
    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: #f8fafc; }
    ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 3px; }
    ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 h-screen flex flex-col overflow-hidden select-none">

  <!-- Header -->
  <header class="h-14 bg-white border-b border-slate-200 px-3 flex items-center justify-between gap-2 shrink-0">
    <!-- Left Controls -->
    <div class="flex items-center gap-2 min-w-0">
      <button id="toggleSidebarBtn" title="プロジェクト一覧の開閉" class="p-2 text-slate-600 hover:bg-slate-100 rounded-md transition-colors">
        <svg class="w-5 h-5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16"/>
        </svg>
      </button>

      <h1 class="text-lg font-bold tracking-tight text-purple-700 whitespace-nowrap mr-1">HTML Box</h1>

      <button id="newProjectBtn" class="bg-purple-700 hover:bg-purple-800 text-white text-xs font-medium px-2.5 py-1.5 rounded-md flex items-center gap-1 transition-colors whitespace-nowrap">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 4v16m8-8H4"/>
        </svg>
        新規作成
      </button>

      <button id="deleteAllBtn" class="bg-white hover:bg-red-50 text-red-600 border border-red-200 text-xs font-medium px-2.5 py-1.5 rounded-md transition-colors whitespace-nowrap">
        全削除
      </button>
    </div>

    <!-- Center JSBin Pane Toggles -->
    <div class="flex items-center bg-slate-100 p-1 rounded-lg border border-slate-200 shrink-0">
      <button data-pane="html" class="pane-toggle px-3 py-1 text-xs font-semibold rounded-md transition-colors bg-purple-700 text-white">HTML</button>
      <button data-pane="css" class="pane-toggle px-3 py-1 text-xs font-semibold rounded-md transition-colors text-slate-600 hover:text-slate-900">CSS</button>
      <button data-pane="js" class="pane-toggle px-3 py-1 text-xs font-semibold rounded-md transition-colors text-slate-600 hover:text-slate-900">JS</button>
      <button data-pane="preview" class="pane-toggle px-3 py-1 text-xs font-semibold rounded-md transition-colors bg-purple-700 text-white">Preview</button>
    </div>

    <!-- Right Actions & Metrics -->
    <div class="flex items-center gap-2 shrink-0">
      <div class="hidden sm:flex items-center gap-1.5 bg-slate-100 border border-slate-200 px-2.5 py-1 rounded-md text-xs text-slate-700">
        <span class="w-2 h-2 rounded-full bg-purple-600"></span>
        <span>Open:</span>
        <strong id="headerOpenCount" class="font-bold text-slate-900">0</strong>
      </div>

      <button id="openTabBtn" class="bg-slate-800 hover:bg-slate-900 text-white text-xs font-medium px-3 py-1.5 rounded-md flex items-center gap-1.5 transition-colors whitespace-nowrap">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"/>
        </svg>
        別タブで開く
      </button>

      <button id="shareUrlBtn" class="bg-purple-700 hover:bg-purple-800 text-white text-xs font-medium px-3 py-1.5 rounded-md flex items-center gap-1.5 transition-colors whitespace-nowrap">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z"/>
        </svg>
        共有リンク作成
      </button>

      <button id="downloadHtmlBtn" class="bg-white hover:bg-slate-100 text-slate-700 border border-slate-300 text-xs font-medium px-2.5 py-1.5 rounded-md transition-colors whitespace-nowrap">
        保存(.html)
      </button>
    </div>
  </header>

  <!-- Main Workspace -->
  <div class="flex-1 flex min-h-0 overflow-hidden">

    <!-- Sidebar -->
    <aside id="sidebar" class="w-64 bg-white border-r border-slate-200 flex flex-col shrink-0">
      <div class="px-3 py-2.5 border-b border-slate-200 flex items-center justify-between bg-slate-50">
        <span class="text-xs font-bold uppercase tracking-wider text-slate-500">プロジェクト一覧</span>
        <label class="text-xs text-purple-700 hover:text-purple-800 font-medium cursor-pointer">
          読込
          <input type="file" id="importHtmlInput" accept=".html,.htm" class="hidden">
        </label>
      </div>

      <div id="projectList" class="flex-1 overflow-y-auto p-1.5 space-y-1"></div>

      <div class="p-3 border-t border-slate-200 bg-slate-50 text-[11px] text-slate-500 leading-relaxed">
        データはブラウザ内に自動保存されます。共有リンクで他の端末へ送信可能です。
      </div>
    </aside>

    <!-- Editor & Preview Panes -->
    <main id="workspace" class="flex-1 flex min-w-0 min-h-0 bg-slate-200 gap-px overflow-hidden">

      <!-- HTML Pane -->
      <section id="pane-html" class="flex-1 min-w-0 flex flex-col bg-white">
        <div class="h-8 px-3 border-b border-slate-200 bg-slate-50 flex items-center justify-between shrink-0">
          <span class="text-xs font-bold text-slate-700">HTML</span>
          <span class="text-[11px] text-slate-400">index.html</span>
        </div>
        <textarea id="htmlEditor" spellcheck="false" class="flex-1 w-full p-3 text-sm text-slate-800 bg-white resize-none focus:outline-none select-text leading-relaxed" placeholder="HTMLを入力..."></textarea>
      </section>

      <!-- CSS Pane -->
      <section id="pane-css" class="hidden flex-1 min-w-0 flex flex-col bg-white">
        <div class="h-8 px-3 border-b border-slate-200 bg-slate-50 flex items-center justify-between shrink-0">
          <span class="text-xs font-bold text-slate-700">CSS</span>
          <span class="text-[11px] text-slate-400">style.css</span>
        </div>
        <textarea id="cssEditor" spellcheck="false" class="flex-1 w-full p-3 text-sm text-slate-800 bg-white resize-none focus:outline-none select-text leading-relaxed" placeholder="CSSを入力..."></textarea>
      </section>

      <!-- JS Pane -->
      <section id="pane-js" class="hidden flex-1 min-w-0 flex flex-col bg-white">
        <div class="h-8 px-3 border-b border-slate-200 bg-slate-50 flex items-center justify-between shrink-0">
          <span class="text-xs font-bold text-slate-700">JavaScript</span>
          <span class="text-[11px] text-slate-400">script.js</span>
        </div>
        <textarea id="jsEditor" spellcheck="false" class="flex-1 w-full p-3 text-sm text-slate-800 bg-white resize-none focus:outline-none select-text leading-relaxed" placeholder="JavaScriptを入力..."></textarea>
      </section>

      <!-- Preview Pane -->
      <section id="pane-preview" class="flex-1 min-w-0 flex flex-col bg-white">
        <div class="h-8 px-3 border-b border-slate-200 bg-slate-50 flex items-center justify-between shrink-0">
          <span class="text-xs font-bold text-slate-700">Preview</span>
          <button id="refreshPreviewBtn" class="text-[11px] text-purple-700 hover:text-purple-800 font-medium">再読み込み</button>
        </div>
        <iframe id="previewFrame" class="flex-1 w-full h-full border-0 bg-white" sandbox="allow-scripts allow-forms allow-modals allow-popups allow-same-origin"></iframe>
      </section>

    </main>
  </div>

  <!-- Confirmation Modal -->
  <div id="confirmModal" class="hidden fixed inset-0 bg-slate-900/40 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-lg border border-slate-200 max-w-sm w-full p-5 shadow-lg">
      <h3 id="modalTitle" class="text-base font-bold text-slate-900 mb-2">確認</h3>
      <p id="modalMessage" class="text-sm text-slate-600 mb-5">本当に実行しますか？</p>
      <div class="flex justify-end gap-2">
        <button id="modalCancelBtn" class="px-3 py-1.5 text-xs font-medium text-slate-700 bg-slate-100 hover:bg-slate-200 rounded-md transition-colors">キャンセル</button>
        <button id="modalConfirmBtn" class="px-3 py-1.5 text-xs font-medium text-white bg-red-600 hover:bg-red-700 rounded-md transition-colors">削除する</button>
      </div>
    </div>
  </div>

  <!-- Rename Modal -->
  <div id="renameModal" class="hidden fixed inset-0 bg-slate-900/40 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-lg border border-slate-200 max-w-sm w-full p-5 shadow-lg">
      <h3 class="text-base font-bold text-slate-900 mb-3">プロジェクト名の変更</h3>
      <input type="text" id="renameInput" class="w-full px-3 py-2 text-sm border border-slate-300 rounded-md focus:outline-none focus:border-purple-600 mb-4 select-text">
      <div class="flex justify-end gap-2">
        <button id="renameCancelBtn" class="px-3 py-1.5 text-xs font-medium text-slate-700 bg-slate-100 hover:bg-slate-200 rounded-md transition-colors">キャンセル</button>
        <button id="renameSaveBtn" class="px-3 py-1.5 text-xs font-medium text-white bg-purple-700 hover:bg-purple-800 rounded-md transition-colors">保存</button>
      </div>
    </div>
  </div>

  <!-- Share URL Modal -->
  <div id="shareModal" class="hidden fixed inset-0 bg-slate-900/40 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-lg border border-slate-200 max-w-md w-full p-5 shadow-lg">
      <h3 class="text-base font-bold text-slate-900 mb-2">共有リンク</h3>
      <p class="text-xs text-slate-500 mb-3">このURLを開くと、現在のHTML/CSS/JSがそのまま復元されます。</p>
      <div class="flex gap-2 mb-4">
        <input type="text" id="shareUrlInput" readonly class="flex-1 px-3 py-1.5 text-xs bg-slate-50 border border-slate-300 rounded-md text-slate-700 select-all focus:outline-none">
        <button id="copyShareUrlBtn" class="px-3 py-1.5 text-xs font-medium text-white bg-purple-700 hover:bg-purple-800 rounded-md transition-colors whitespace-nowrap">コピー</button>
      </div>
      <div class="flex justify-end">
        <button id="shareCloseBtn" class="px-3 py-1.5 text-xs font-medium text-slate-700 bg-slate-100 hover:bg-slate-200 rounded-md transition-colors">閉じる</button>
      </div>
    </div>
  </div>

  <!-- Toast Notification -->
  <div id="toast" class="hidden fixed bottom-4 right-4 bg-slate-900 text-white text-xs font-medium px-4 py-2.5 rounded-md shadow-md z-50"></div>

  <script>
    const STORAGE_KEY = 'html_box_projects_v1';
    const ACTIVE_KEY = 'html_box_active_id_v1';
    const PANES_KEY = 'html_box_panes_v1';

    let projects = [];
    let activeProjectId = null;
    let visiblePanes = { html: true, css: false, js: false, preview: true };
    let sidebarOpen = true;
    let pendingConfirmAction = null;
    let renamingProjectId = null;

    const channel = new BroadcastChannel('html_box_sync');

    function showToast(msg) {
      const toast = document.getElementById('toast');
      toast.textContent = msg;
      toast.classList.remove('hidden');
      clearTimeout(window._toastTimer);
      window._toastTimer = setTimeout(() => toast.classList.add('hidden'), 2400);
    }

    function getSmallestProjectName() {
      const usedNumbers = new Set();
      projects.forEach(p => {
        const match = p.name.match(/^Project\s+(\d+)$/);
        if (match) usedNumbers.add(parseInt(match[1], 10));
      });
      let n = 1;
      while (usedNumbers.has(n)) n++;
      return `Project ${n}`;
    }

    function createDefaultProject(customName, html = 'Hello, World!', css = '', js = '') {
      return {
        id: 'proj_' + Date.now() + '_' + Math.random().toString(36).slice(2, 7),
        name: customName || getSmallestProjectName(),
        html,
        css,
        js,
        views: 0,
        updatedAt: Date.now()
      };
    }

    function saveState(broadcast = true) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(projects));
      localStorage.setItem(ACTIVE_KEY, activeProjectId || '');
      localStorage.setItem(PANES_KEY, JSON.stringify(visiblePanes));
      if (broadcast) {
        channel.postMessage({ type: 'sync' });
      }
    }

    function loadState() {
      try {
        const savedProjects = JSON.parse(localStorage.getItem(STORAGE_KEY));
        if (Array.isArray(savedProjects) && savedProjects.length > 0) {
          projects = savedProjects;
        }
        const savedActive = localStorage.getItem(ACTIVE_KEY);
        if (savedActive && projects.some(p => p.id === savedActive)) {
          activeProjectId = savedActive;
        }
        const savedPanes = JSON.parse(localStorage.getItem(PANES_KEY));
        if (savedPanes) {
          visiblePanes = { ...visiblePanes, ...savedPanes };
        }
      } catch (e) {}

      if (projects.length === 0) {
        const initial = createDefaultProject('Project 1', 'Hello, World!', '', '');
        projects.push(initial);
        activeProjectId = initial.id;
        saveState(false);
      } else if (!activeProjectId) {
        activeProjectId = projects[0].id;
      }
    }

    function handleSharedHash() {
      const hash = window.location.hash;
      if (hash && hash.startsWith('#share=')) {
        try {
          const encoded = hash.slice(7);
          const jsonStr = LZString.decompressFromEncodedURIComponent(encoded);
          if (jsonStr) {
            const data = JSON.parse(jsonStr);
            const sharedProj = createDefaultProject(
              data.name || getSmallestProjectName(),
              data.html ?? 'Hello, World!',
              data.css ?? '',
              data.js ?? ''
            );
            sharedProj.views = 1;
            projects.unshift(sharedProj);
            activeProjectId = sharedProj.id;
            if (data.panes) {
              visiblePanes = { ...visiblePanes, ...data.panes };
            }
            history.replaceState(null, '', window.location.pathname + window.location.search);
            saveState();
            showToast('共有プロジェクトを読み込みました');
          }
        } catch (e) {
          showToast('共有リンクの読み込みに失敗しました');
        }
      }
    }

    function getActiveProject() {
      return projects.find(p => p.id === activeProjectId) || projects[0];
    }

    function buildCombinedDocument(proj) {
      const html = proj.html || '';
      const css = proj.css || '';
      const js = proj.js || '';

      const styleTag = css.trim() ? `<style>\n${css}\n</style>` : '';
      const scriptTag = js.trim() ? `<script>\n${js}\n<\/script>` : '';

      if (/<html[\s>]/i.test(html)) {
        let doc = html;
        if (styleTag) {
          doc = /<\/head>/i.test(doc)
            ? doc.replace(/<\/head>/i, `${styleTag}\n</head>`)
            : `${styleTag}\n${doc}`;
        }
        if (scriptTag) {
          doc = /<\/body>/i.test(doc)
            ? doc.replace(/<\/body>/i, `${scriptTag}\n</body>`)
            : `${doc}\n${scriptTag}`;
        }
        return doc;
      }

      return `<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>${escapeHtml(proj.name)}</title>
  ${styleTag}
</head>
<body>
${html}
${scriptTag}
</body>
</html>`;
    }

    function escapeHtml(str) {
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;');
    }

    function updatePreview() {
      const proj = getActiveProject();
      if (!proj) return;
      const frame = document.getElementById('previewFrame');
      frame.srcdoc = buildCombinedDocument(proj);
    }

    function renderPanes() {
      ['html', 'css', 'js', 'preview'].forEach(key => {
        const btn = document.querySelector(`.pane-toggle[data-pane="${key}"]`);
        const pane = document.getElementById(`pane-${key}`);
        if (visiblePanes[key]) {
          btn.classList.add('bg-purple-700', 'text-white');
          btn.classList.remove('text-slate-600', 'hover:text-slate-900');
          pane.classList.remove('hidden');
        } else {
          btn.classList.remove('bg-purple-700', 'text-white');
          btn.classList.add('text-slate-600', 'hover:text-slate-900');
          pane.classList.add('hidden');
        }
      });
    }

    function renderEditors() {
      const proj = getActiveProject();
      if (!proj) return;
      document.getElementById('htmlEditor').value = proj.html;
      document.getElementById('cssEditor').value = proj.css;
      document.getElementById('jsEditor').value = proj.js;
      document.getElementById('headerOpenCount').textContent = proj.views || 0;
      updatePreview();
    }

    function renderProjectList() {
      const listEl = document.getElementById('projectList');
      listEl.innerHTML = '';

      projects.forEach(proj => {
        const isActive = proj.id === activeProjectId;
        const item = document.createElement('div');
        item.className = `flex items-center justify-between gap-1.5 px-2.5 py-2 rounded-md border border-transparent cursor-pointer transition-colors ${
          isActive ? 'bg-purple-50 text-purple-900 font-semibold' : 'text-slate-700 hover:bg-slate-100'
        }`;

        item.innerHTML = `
          <div class="flex items-center gap-2 min-w-0 flex-1" data-select="${proj.id}">
            <span class="w-2 h-2 rounded-full shrink-0 ${isActive ? 'bg-purple-700' : 'bg-slate-300'}"></span>
            <span class="text-xs truncate">${escapeHtml(proj.name)}</span>
            <span class="text-[10px] px-1.5 py-0.5 rounded bg-slate-200/80 text-slate-600 font-mono shrink-0" title="Open数">${proj.views || 0}</span>
          </div>
          <div class="flex items-center gap-0.5 shrink-0">
            <button type="button" data-rename="${proj.id}" title="名前変更" class="p-1 text-slate-400 hover:text-slate-700 rounded transition-colors">
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/>
              </svg>
            </button>
            <button type="button" data-duplicate="${proj.id}" title="複製" class="p-1 text-slate-400 hover:text-slate-700 rounded transition-colors">
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"/>
              </svg>
            </button>
            <button type="button" data-delete="${proj.id}" title="削除" class="p-1 text-slate-400 hover:text-red-600 rounded transition-colors">
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>
              </svg>
            </button>
          </div>
        `;

        listEl.appendChild(item);
      });
    }

    function openConfirmModal(title, message, onConfirm) {
      document.getElementById('modalTitle').textContent = title;
      document.getElementById('modalMessage').textContent = message;
      pendingConfirmAction = onConfirm;
      document.getElementById('confirmModal').classList.remove('hidden');
    }

    function closeConfirmModal() {
      document.getElementById('confirmModal').classList.add('hidden');
      pendingConfirmAction = null;
    }

    // Event Listeners
    document.getElementById('toggleSidebarBtn').addEventListener('click', () => {
      sidebarOpen = !sidebarOpen;
      document.getElementById('sidebar').classList.toggle('hidden', !sidebarOpen);
    });

    document.getElementById('newProjectBtn').addEventListener('click', () => {
      const proj = createDefaultProject();
      projects.unshift(proj);
      activeProjectId = proj.id;
      saveState();
      renderProjectList();
      renderEditors();
      showToast(`${proj.name} を作成しました`);
    });

    document.getElementById('deleteAllBtn').addEventListener('click', () => {
      openConfirmModal(
        '全プロジェクトの削除',
        '保存されているすべてのプロジェクトを削除しますか？この操作は取り消せません。',
        () => {
          const fresh = createDefaultProject('Project 1', 'Hello, World!', '', '');
          projects = [fresh];
          activeProjectId = fresh.id;
          saveState();
          renderProjectList();
          renderEditors();
          showToast('すべて削除しました');
        }
      );
    });

    document.querySelectorAll('.pane-toggle').forEach(btn => {
      btn.addEventListener('click', () => {
        const key = btn.dataset.pane;
        const activeCount = Object.values(visiblePanes).filter(Boolean).length;
        if (visiblePanes[key] && activeCount === 1) {
          showToast('最低1つのパネルを表示する必要があります');
          return;
        }
        visiblePanes[key] = !visiblePanes[key];
        saveState(false);
        renderPanes();
      });
    });

    ['html', 'css', 'js'].forEach(type => {
      const editor = document.getElementById(`${type}Editor`);
      editor.addEventListener('input', () => {
        const proj = getActiveProject();
        if (!proj) return;
        proj[type] = editor.value;
        proj.updatedAt = Date.now();
        saveState();
        clearTimeout(window._previewTimer);
        window._previewTimer = setTimeout(updatePreview, 150);
      });

      editor.addEventListener('keydown', e => {
        if (e.key === 'Tab') {
          e.preventDefault();
          const start = editor.selectionStart;
          const end = editor.selectionEnd;
          editor.value = editor.value.substring(0, start) + '  ' + editor.value.substring(end);
          editor.selectionStart = editor.selectionEnd = start + 2;
          editor.dispatchEvent(new Event('input'));
        }
      });
    });

    document.getElementById('refreshPreviewBtn').addEventListener('click', updatePreview);

    document.getElementById('openTabBtn').addEventListener('click', () => {
      const proj = getActiveProject();
      if (!proj) return;
      proj.views = (proj.views || 0) + 1;
      saveState();
      renderProjectList();
      document.getElementById('headerOpenCount').textContent = proj.views;

      const doc = buildCombinedDocument(proj);
      const blob = new Blob([doc], { type: 'text/html' });
      const url = URL.createObjectURL(blob);
      window.open(url, '_blank');
    });

    document.getElementById('shareUrlBtn').addEventListener('click', () => {
      const proj = getActiveProject();
      if (!proj) return;
      const payload = JSON.stringify({
        name: proj.name,
        html: proj.html,
        css: proj.css,
        js: proj.js,
        panes: visiblePanes
      });
      const compressed = LZString.compressToEncodedURIComponent(payload);
      const baseUrl = window.location.href.split('#')[0];
      const shareUrl = `${baseUrl}#share=${compressed}`;
      document.getElementById('shareUrlInput').value = shareUrl;
      document.getElementById('shareModal').classList.remove('hidden');
    });

    document.getElementById('copyShareUrlBtn').addEventListener('click', () => {
      const input = document.getElementById('shareUrlInput');
      input.select();
      navigator.clipboard.writeText(input.value).then(() => {
        showToast('共有リンクをコピーしました');
      }).catch(() => {
        document.execCommand('copy');
        showToast('共有リンクをコピーしました');
      });
    });

    document.getElementById('shareCloseBtn').addEventListener('click', () => {
      document.getElementById('shareModal').classList.add('hidden');
    });

    document.getElementById('downloadHtmlBtn').addEventListener('click', () => {
      const proj = getActiveProject();
      if (!proj) return;
      proj.views = (proj.views || 0) + 1;
      saveState();
      renderProjectList();
      document.getElementById('headerOpenCount').textContent = proj.views;

      const doc = buildCombinedDocument(proj);
      const blob = new Blob([doc], { type: 'text/html' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = `${proj.name.replace(/[^a-zA-Z0-9_\-\u3040-\u309F\u30A0-\u30FF\u4E00-\u9FAF]/g, '_')}.html`;
      a.click();
      showToast('HTMLファイルを保存しました');
    });

    document.getElementById('importHtmlInput').addEventListener('change', e => {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = ev => {
        const content = ev.target.result;
        const name = file.name.replace(/\.(html|htm)$/i, '') || getSmallestProjectName();
        const proj = createDefaultProject(name, content, '', '');
        projects.unshift(proj);
        activeProjectId = proj.id;
        saveState();
        renderProjectList();
        renderEditors();
        showToast(`${name} を読み込みました`);
      };
      reader.readAsText(file);
      e.target.value = '';
    });

    document.getElementById('projectList').addEventListener('click', e => {
      const delBtn = e.target.closest('[data-delete]');
      if (delBtn) {
        const id = delBtn.dataset.delete;
        const target = projects.find(p => p.id === id);
        if (!target) return;
        openConfirmModal(
          'プロジェクトの削除',
          `「${target.name}」を削除しますか？`,
          () => {
            projects = projects.filter(p => p.id !== id);
            if (projects.length === 0) {
              const fresh = createDefaultProject('Project 1', 'Hello, World!', '', '');
              projects.push(fresh);
            }
            if (!projects.some(p => p.id === activeProjectId)) {
              activeProjectId = projects[0].id;
            }
            saveState();
            renderProjectList();
            renderEditors();
            showToast('削除しました');
          }
        );
        return;
      }

      const dupBtn = e.target.closest('[data-duplicate]');
      if (dupBtn) {
        const id = dupBtn.dataset.duplicate;
        const target = projects.find(p => p.id === id);
        if (!target) return;
        const copy = createDefaultProject(getSmallestProjectName(), target.html, target.css, target.js);
        projects.unshift(copy);
        activeProjectId = copy.id;
        saveState();
        renderProjectList();
        renderEditors();
        showToast(`${copy.name} として複製しました`);
        return;
      }

      const renBtn = e.target.closest('[data-rename]');
      if (renBtn) {
        const id = renBtn.dataset.rename;
        const target = projects.find(p => p.id === id);
        if (!target) return;
        renamingProjectId = id;
        const input = document.getElementById('renameInput');
        input.value = target.name;
        document.getElementById('renameModal').classList.remove('hidden');
        input.focus();
        input.select();
        return;
      }

      const selEl = e.target.closest('[data-select]');
      if (selEl) {
        activeProjectId = selEl.dataset.select;
        saveState();
        renderProjectList();
        renderEditors();
      }
    });

    document.getElementById('modalCancelBtn').addEventListener('click', closeConfirmModal);
    document.getElementById('modalConfirmBtn').addEventListener('click', () => {
      if (pendingConfirmAction) pendingConfirmAction();
      closeConfirmModal();
    });

    document.getElementById('renameCancelBtn').addEventListener('click', () => {
      document.getElementById('renameModal').classList.add('hidden');
      renamingProjectId = null;
    });

    document.getElementById('renameSaveBtn').addEventListener('click', () => {
      const newName = document.getElementById('renameInput').value.trim();
      if (newName && renamingProjectId) {
        const target = projects.find(p => p.id === renamingProjectId);
        if (target) {
          target.name = newName;
          saveState();
          renderProjectList();
          showToast('名前を変更しました');
        }
      }
      document.getElementById('renameModal').classList.add('hidden');
      renamingProjectId = null;
    });

    document.getElementById('renameInput').addEventListener('keydown', e => {
      if (e.key === 'Enter') document.getElementById('renameSaveBtn').click();
      if (e.key === 'Escape') document.getElementById('renameCancelBtn').click();
    });

    // Cross-tab real-time sync
    function syncFromStorage() {
      try {
        const latest = JSON.parse(localStorage.getItem(STORAGE_KEY));
        if (Array.isArray(latest) && latest.length > 0) {
          projects = latest;
          if (!projects.some(p => p.id === activeProjectId)) {
            activeProjectId = projects[0].id;
          }
          renderProjectList();
          const current = getActiveProject();
          if (current) {
            document.getElementById('headerOpenCount').textContent = current.views || 0;
          }
        }
      } catch (e) {}
    }

    window.addEventListener('storage', e => {
      if (e.key === STORAGE_KEY) syncFromStorage();
    });
    channel.onmessage = () => syncFromStorage();

    // Init
    loadState();
    handleSharedHash();
    renderPanes();
    renderProjectList();
    renderEditors();
  </script>
</body>
</html>
