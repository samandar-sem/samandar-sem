<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ombor — ism bilan kirish va odamlar bazasi</title>
<meta name="description" content="Ism bilan kirish, profil rasmi, layk/dislayk va odamlar ombori (qidiruv, qo'shish, tahrirlash) bilan ishlaydigan veb-ilova.">
<meta name="robots" content="index, follow">
<meta property="og:title" content="Ombor">
<meta property="og:description" content="Ism bilan kirish, profil va odamlar bazasini boshqarish uchun veb-ilova.">
<meta property="og:type" content="website">
<meta name="theme-color" content="#14171c">
<style>
  :root{
    --bg:#14171c;
    --card:#1c2129;
    --card2:#232a34;
    --line:#2c333d;
    --gold:#e2a73e;
    --gold-dark:#3a2c14;
    --teal:#2fbf9e;
    --teal-dark:#123a30;
    --red:#e2574a;
    --red-dark:#3a1a16;
    --text:#eef1f5;
    --text2:#9aa3b1;
    --text3:#606a78;
  }
  *{box-sizing:border-box;margin:0;padding:0;}
  body{
    background:var(--bg);
    color:var(--text);
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Arial,sans-serif;
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
  }
  .app{width:100%;max-width:420px;}
  .screen{display:none;}
  .screen.active{display:block;animation:fade .25s ease;}
  @keyframes fade{from{opacity:0;transform:translateY(6px);}to{opacity:1;transform:translateY(0);}}

  /* ---------- SCREEN 1: KIRISH ---------- */
  .login-card{
    background:var(--card);
    border:1px solid var(--line);
    border-radius:18px;
    padding:40px 28px;
    text-align:center;
  }
  .login-mark{
    width:56px;height:56px;border-radius:14px;
    background:var(--gold-dark);
    display:flex;align-items:center;justify-content:center;
    margin:0 auto 20px;
    font-size:26px;
  }
  .login-title{font-size:20px;font-weight:600;margin-bottom:6px;}
  .login-sub{color:var(--text2);font-size:14px;margin-bottom:28px;}
  .field-label{
    display:block;text-align:left;font-size:13px;color:var(--text2);
    margin-bottom:8px;
  }
  input[type=text]{
    width:100%;
    background:var(--card2);
    border:1px solid var(--line);
    color:var(--text);
    font-size:15px;
    padding:13px 14px;
    border-radius:10px;
    outline:none;
    margin-bottom:20px;
    transition:border-color .15s;
  }
  input[type=text]:focus{border-color:var(--gold);}
  .btn{
    width:100%;
    border:none;
    border-radius:10px;
    padding:14px;
    font-size:15px;
    font-weight:600;
    cursor:pointer;
    transition:transform .1s, opacity .15s;
  }
  .btn:active{transform:scale(.98);}
  .btn-gold{background:var(--gold);color:#241902;}
  .btn-gold:disabled{opacity:.4;cursor:not-allowed;}
  .hint{margin-top:14px;font-size:12px;color:var(--text3);}

  /* ---------- TOP BAR (screens 2 & 3) ---------- */
  .topbar{
    display:flex;align-items:center;justify-content:space-between;
    margin-bottom:20px;
  }
  .who{display:flex;align-items:center;gap:12px;}
  .avatar{
    width:40px;height:40px;border-radius:50%;
    background:var(--gold-dark);color:var(--gold);
    display:flex;align-items:center;justify-content:center;
    font-weight:600;font-size:15px;flex-shrink:0;
  }
  .who-name{font-size:16px;font-weight:600;}
  .who-role{font-size:12px;color:var(--text2);}
  .icon-btn{
    width:38px;height:38px;border-radius:10px;
    background:var(--card2);border:1px solid var(--line);
    color:var(--text);font-size:18px;
    display:flex;align-items:center;justify-content:center;
    cursor:pointer;
  }
  .icon-btn:hover{border-color:var(--gold);}

  /* ---------- SCREEN 2: PROFIL ---------- */
  .photo-box{
    width:100%;
    aspect-ratio:1/1;
    background:var(--card2);
    border:1px solid var(--line);
    border-radius:16px;
    display:flex;align-items:center;justify-content:center;
    overflow:hidden;
    cursor:pointer;
    margin-bottom:16px;
    position:relative;
  }
  .photo-box img{width:100%;height:100%;object-fit:cover;}
  .photo-placeholder{color:var(--text3);font-size:13px;text-align:center;padding:20px;}
  .photo-placeholder .big{font-size:32px;display:block;margin-bottom:8px;}

  .reactions{
    display:flex;gap:10px;margin-bottom:16px;
  }
  .react-btn{
    flex:1;
    display:flex;align-items:center;justify-content:center;gap:8px;
    background:var(--card2);
    border:1px solid var(--line);
    color:var(--text2);
    padding:12px;
    border-radius:10px;
    font-size:14px;
    font-weight:600;
    cursor:pointer;
  }
  .react-btn.like.on{background:var(--teal-dark);border-color:var(--teal);color:var(--teal);}
  .react-btn.dislike.on{background:var(--red-dark);border-color:var(--red);color:var(--red);}

  .likers{font-size:12px;color:var(--text2);margin:-8px 0 16px;min-height:16px;}
  .likers b{color:var(--teal);font-weight:600;}

  .ombor-avatar-btn{
    display:flex;flex-direction:column;align-items:center;gap:8px;
    background:none;border:none;cursor:pointer;padding:8px;
  }
  .ombor-avatar{
    width:72px;height:72px;border-radius:50%;
    background:var(--card);border:1px solid var(--line);
    display:flex;align-items:center;justify-content:center;
    font-size:32px;transition:border-color .15s, transform .1s;
  }
  .ombor-avatar-btn:hover .ombor-avatar{border-color:var(--gold);}
  .ombor-avatar-btn:active .ombor-avatar{transform:scale(.96);}
  .ombor-avatar-label{font-size:13px;color:var(--text2);font-weight:600;}

  /* ---------- SCREEN 3: OMBOR ---------- */
  .empty{
    text-align:center;color:var(--text3);font-size:13px;
    padding:40px 20px;border:1px dashed var(--line);border-radius:14px;
  }
  .grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
  .entry{
    background:var(--card);border:1px solid var(--line);
    border-radius:14px;overflow:hidden;
  }
  .entry-photo{width:100%;aspect-ratio:1/1;background:var(--card2);overflow:hidden;position:relative;}
  .entry-photo img{width:100%;height:100%;object-fit:cover;}
  .entry-photo .ph{width:100%;height:100%;display:flex;align-items:center;justify-content:center;color:var(--text3);font-size:22px;}
  .entry-photo-del{
    position:absolute;top:6px;right:6px;
    width:26px;height:26px;border-radius:8px;
    background:rgba(20,23,28,.85);border:1px solid var(--line);
    color:var(--red);font-size:13px;
    display:flex;align-items:center;justify-content:center;
    cursor:pointer;
  }
  .entry-bottom{display:flex;align-items:center;justify-content:space-between;padding:10px 10px 10px 12px;gap:6px;}
  .entry-name{font-size:13px;font-weight:600;flex:1;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
  .entry-name-input{
    flex:1;min-width:0;background:var(--card2);border:1px solid var(--gold);
    color:var(--text);font-size:13px;border-radius:6px;padding:5px 7px;
  }
  .entry-edit{
    width:26px;height:26px;border-radius:8px;flex-shrink:0;
    background:var(--card2);border:1px solid var(--line);color:var(--text2);
    font-size:12px;display:flex;align-items:center;justify-content:center;cursor:pointer;
  }
  .entry-edit:hover{border-color:var(--gold);color:var(--gold);}

  .search-wrap{position:relative;margin-bottom:16px;}
  .search-wrap input[type=text]{margin-bottom:0;padding-left:38px;}
  .search-wrap .search-icon{
    position:absolute;left:13px;top:50%;transform:translateY(-50%);
    color:var(--text3);font-size:14px;pointer-events:none;
  }

  .back-link{
    display:inline-flex;align-items:center;gap:6px;
    color:var(--text2);font-size:13px;text-decoration:none;
    background:none;border:none;cursor:pointer;padding:0;margin-bottom:18px;
  }

  /* add-entry modal */
  .overlay{
    position:fixed;inset:0;background:rgba(0,0,0,.55);
    display:none;align-items:center;justify-content:center;padding:24px;z-index:10;
  }
  .overlay.active{display:flex;}
  .modal{
    background:var(--card);border:1px solid var(--line);border-radius:16px;
    padding:24px;width:100%;max-width:340px;
  }
  .modal-title{font-size:16px;font-weight:600;margin-bottom:16px;}
  .upload-zone{
    width:100%;aspect-ratio:1/1;background:var(--card2);border:1px dashed var(--line);
    border-radius:12px;display:flex;align-items:center;justify-content:center;
    cursor:pointer;overflow:hidden;margin-bottom:16px;color:var(--text3);font-size:13px;text-align:center;
  }
  .upload-zone img{width:100%;height:100%;object-fit:cover;}
  .modal-actions{display:flex;gap:10px;}
  .btn-ghost{background:var(--card2);color:var(--text2);}
</style>
</head>
<body>

<div class="app">

  <!-- SCREEN 1 : KIRISH -->
  <section class="screen active" id="screen-login">
    <div class="login-card">
      <div class="login-mark">🔑</div>
      <div class="login-title">Xush kelibsiz</div>
      <div class="login-sub">Davom etish uchun ismingizni kiriting</div>
      <label class="field-label" for="name-input">Ism</label>
      <input type="text" id="name-input" placeholder="Ismingizni yozing" autocomplete="off">
      <button class="btn btn-gold" id="btn-kirish" disabled>Kirish</button>
      <div class="hint">Ismingiz saqlanadi va profilingizda ko'rinadi</div>
    </div>
  </section>

  <!-- SCREEN 2 : PROFIL -->
  <section class="screen" id="screen-profile">
    <div class="topbar">
      <div class="who">
        <div class="avatar" id="avatar-letter">?</div>
        <div>
          <div class="who-name" id="profile-name">—</div>
          <div class="who-role" id="profile-role">Profil</div>
        </div>
      </div>
      <button class="icon-btn" id="btn-logout" title="Chiqish">⎋</button>
    </div>

    <div class="photo-box" id="photo-box">
      <div class="photo-placeholder" id="photo-placeholder">
        <span class="big">🖼️</span>
        Rasm qo'yish uchun bosing
      </div>
    </div>
    <input type="file" accept="image/*" id="photo-input" style="display:none;">

    <div class="reactions">
      <button class="react-btn like" id="btn-like">👍 Layk</button>
      <button class="react-btn dislike" id="btn-dislike">👎 Dislayk</button>
    </div>
    <div class="likers" id="likers-text"></div>

    <div style="display:flex;justify-content:center;padding:8px 0 4px;">
      <button class="ombor-avatar-btn" id="btn-ombor" title="Omborga kirish">
        <span class="ombor-avatar">👤</span>
        <span class="ombor-avatar-label">Ombor</span>
      </button>
    </div>
  </section>

  <!-- SCREEN 3 : OMBOR -->
  <section class="screen" id="screen-ombor">
    <button class="back-link" id="btn-back">← Profilga qaytish</button>
    <div class="topbar">
      <div class="who">
        <div class="avatar">🗄️</div>
        <div>
          <div class="who-name">Ombor</div>
          <div class="who-role">Odamlar ro'yxati</div>
        </div>
      </div>
      <button class="icon-btn" id="btn-add" title="Qo'shish">+</button>
    </div>

    <div class="search-wrap">
      <span class="search-icon">🔍</span>
      <input type="text" id="ombor-search" placeholder="Ism bo'yicha qidirish">
    </div>

    <div id="ombor-empty" class="empty">Hozircha hech narsa saqlanmagan.<br>Qo'shish uchun yuqoridagi + tugmasini bosing.</div>
    <div id="ombor-grid" class="grid" style="display:none;"></div>
  </section>

</div>

<!-- ADD ENTRY MODAL -->
<div class="overlay" id="overlay">
  <div class="modal">
    <div class="modal-title">Yangi ma'lumot qo'shish</div>
    <div class="upload-zone" id="modal-upload">
      <span id="modal-upload-text">Rasm qo'yish uchun bosing</span>
    </div>
    <input type="file" accept="image/*" id="modal-photo-input" style="display:none;">
    <label class="field-label" for="modal-name-input">Ism</label>
    <input type="text" id="modal-name-input" placeholder="Ismini yozing" style="margin-bottom:16px;">
    <div class="modal-actions">
      <button class="btn btn-ghost" id="modal-cancel">Bekor qilish</button>
      <button class="btn btn-gold" id="modal-save">Saqlash</button>
    </div>
  </div>
</div>

<script>
  const state = {
    people: [],       // {id, name, photo, likedBy:[names], dislikedBy:[names]}
    currentUser: null, // person object of who is logged in
    viewing: null       // person object currently shown on profile screen
  };

  const $ = id => document.getElementById(id);

  function showScreen(id){
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    $(id).classList.add('active');
  }

  function initials(name){
    return name.trim().slice(0,2).toUpperCase() || '?';
  }

  function findOrCreatePerson(name){
    let p = state.people.find(x => x.name.toLowerCase() === name.toLowerCase());
    if(!p){
      p = { id: Date.now() + Math.random(), name, photo: null, likedBy: [], dislikedBy: [] };
      state.people.push(p);
    }
    return p;
  }

  // ---- SCREEN 1: login ----
  const nameInput = $('name-input');
  const btnKirish = $('btn-kirish');

  nameInput.addEventListener('input', () => {
    btnKirish.disabled = nameInput.value.trim().length === 0;
  });

  nameInput.addEventListener('keydown', e => {
    if(e.key === 'Enter' && !btnKirish.disabled) btnKirish.click();
  });

  btnKirish.addEventListener('click', () => {
    const name = nameInput.value.trim();
    if(!name) return;
    const person = findOrCreatePerson(name);
    state.currentUser = person;
    state.viewing = person;
    renderProfile();
    showScreen('screen-profile');
  });

  // ---- SCREEN 2: profile ----
  function renderProfile(){
    const p = state.viewing;
    const isSelf = p === state.currentUser;

    $('profile-name').textContent = p.name;
    $('avatar-letter').textContent = initials(p.name);
    $('profile-role').textContent = isSelf ? 'Profil' : "Boshqa foydalanuvchi profili";

    $('btn-logout').textContent = isSelf ? '⎋' : '←';
    $('btn-logout').title = isSelf ? 'Chiqish' : "Omborga qaytish";

    if(p.photo){
      $('photo-box').innerHTML = `<img src="${p.photo}" alt="Profil rasmi">`;
    } else {
      $('photo-box').innerHTML = `<div class="photo-placeholder" id="photo-placeholder"><span class="big">🖼️</span>Rasm qo'yish uchun bosing</div>`;
    }

    const likedByMe = state.currentUser && p.likedBy.includes(state.currentUser.name);
    const dislikedByMe = state.currentUser && p.dislikedBy.includes(state.currentUser.name);
    $('btn-like').classList.toggle('on', !!likedByMe);
    $('btn-dislike').classList.toggle('on', !!dislikedByMe);

    $('likers-text').innerHTML = p.likedBy.length
      ? `Yoqtirganlar: <b>${p.likedBy.join(', ')}</b>`
      : '';
  }

  $('btn-logout').addEventListener('click', () => {
    if(state.viewing === state.currentUser){
      state.currentUser = null;
      state.viewing = null;
      showScreen('screen-login');
      nameInput.value = '';
      btnKirish.disabled = true;
    } else {
      state.viewing = state.currentUser;
      showScreen('screen-ombor');
      renderOmbor();
    }
  });

  $('photo-box').addEventListener('click', () => $('photo-input').click());
  $('photo-input').addEventListener('change', e => {
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      state.viewing.photo = ev.target.result;
      renderProfile();
    };
    reader.readAsDataURL(file);
  });

  $('btn-like').addEventListener('click', () => {
    if(!state.currentUser) return;
    const p = state.viewing;
    const me = state.currentUser.name;
    if(p.likedBy.includes(me)){
      p.likedBy = p.likedBy.filter(n => n !== me);
    } else {
      p.likedBy.push(me);
      p.dislikedBy = p.dislikedBy.filter(n => n !== me);
    }
    renderProfile();
  });
  $('btn-dislike').addEventListener('click', () => {
    if(!state.currentUser) return;
    const p = state.viewing;
    const me = state.currentUser.name;
    if(p.dislikedBy.includes(me)){
      p.dislikedBy = p.dislikedBy.filter(n => n !== me);
    } else {
      p.dislikedBy.push(me);
      p.likedBy = p.likedBy.filter(n => n !== me);
    }
    renderProfile();
  });

  $('btn-ombor').addEventListener('click', () => {
    showScreen('screen-ombor');
    renderOmbor();
  });

  $('btn-back').addEventListener('click', () => {
    state.viewing = state.currentUser;
    renderProfile();
    showScreen('screen-profile');
  });

  // ---- SCREEN 3: ombor ----
  const searchInput = $('ombor-search');
  searchInput.addEventListener('input', renderOmbor);

  function renderOmbor(){
    const grid = $('ombor-grid');
    const empty = $('ombor-empty');
    const q = searchInput.value.trim().toLowerCase();
    const list = q
      ? state.people.filter(p => p.name.toLowerCase().includes(q))
      : state.people;

    if(state.people.length === 0){
      empty.textContent = "Hozircha hech narsa saqlanmagan.";
      empty.style.display = 'block';
      grid.style.display = 'none';
      return;
    }
    if(list.length === 0){
      empty.textContent = "Hech kim topilmadi.";
      empty.style.display = 'block';
      grid.style.display = 'none';
      return;
    }
    empty.style.display = 'none';
    grid.style.display = 'grid';
    grid.innerHTML = list.map(p => `
      <div class="entry" data-open="${p.id}">
        <div class="entry-photo">
          ${p.photo
            ? `<img src="${p.photo}" alt="${p.name}"><button class="entry-photo-del" data-del-photo="${p.id}" title="Rasmni o'chirish">🗑</button>`
            : `<div class="ph">👤</div>`}
        </div>
        <div class="entry-bottom">
          <span class="entry-name" id="entry-name-${p.id}">${p.name}</span>
          <button class="entry-edit" data-edit-name="${p.id}" title="Ismni o'zgartirish">✎</button>
        </div>
      </div>
    `).join('');

    grid.querySelectorAll('[data-open]').forEach(card => {
      card.addEventListener('click', e => {
        if(e.target.closest('[data-del-photo]') || e.target.closest('[data-edit-name]')) return;
        const person = state.people.find(x => x.id == card.dataset.open);
        state.viewing = person;
        renderProfile();
        showScreen('screen-profile');
      });
    });

    grid.querySelectorAll('[data-del-photo]').forEach(btn => {
      btn.addEventListener('click', () => {
        const person = state.people.find(x => x.id == btn.dataset.delPhoto);
        person.photo = null;
        renderOmbor();
      });
    });

    grid.querySelectorAll('[data-edit-name]').forEach(btn => {
      btn.addEventListener('click', () => startNameEdit(btn.dataset.editName));
    });
  }

  function startNameEdit(id){
    const person = state.people.find(x => x.id == id);
    const nameEl = $(`entry-name-${id}`);
    const current = person.name;
    const input = document.createElement('input');
    input.type = 'text';
    input.className = 'entry-name-input';
    input.value = current;
    nameEl.replaceWith(input);
    input.focus();
    input.select();

    const save = () => {
      const val = input.value.trim();
      person.name = val || current;
      renderOmbor();
    };
    input.addEventListener('blur', save);
    input.addEventListener('keydown', e => {
      if(e.key === 'Enter') input.blur();
      if(e.key === 'Escape'){ input.value = current; input.blur(); }
    });
  }

  // ---- ADD ENTRY MODAL ----
  let modalPhoto = null;

  $('btn-add').addEventListener('click', () => {
    modalPhoto = null;
    $('modal-name-input').value = '';
    $('modal-upload').innerHTML = '<span id="modal-upload-text">Rasm qo\'yish uchun bosing</span>';
    $('overlay').classList.add('active');
  });

  $('modal-cancel').addEventListener('click', () => {
    $('overlay').classList.remove('active');
  });
  $('overlay').addEventListener('click', e => {
    if(e.target === $('overlay')) $('overlay').classList.remove('active');
  });

  $('modal-upload').addEventListener('click', () => $('modal-photo-input').click());
  $('modal-photo-input').addEventListener('change', e => {
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      modalPhoto = ev.target.result;
      $('modal-upload').innerHTML = `<img src="${modalPhoto}" alt="Yangi rasm">`;
    };
    reader.readAsDataURL(file);
  });

  $('modal-save').addEventListener('click', () => {
    const name = $('modal-name-input').value.trim();
    if(!name) { $('modal-name-input').focus(); return; }
    const person = findOrCreatePerson(name);
    if(modalPhoto) person.photo = modalPhoto;
    $('overlay').classList.remove('active');
    renderOmbor();
  });
</script>

</body>
</html>
