<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Safe Thumbnail with Play</title>
  <style>
    :root{ --size:516px; --blur:12px; --radius:12px; }
    body{ font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial; display:flex; align-items:center; justify-content:center; min-height:100vh; background:#f3f4f6; margin:0; }
    .thumb {
      position:relative;
      width:var(--size);
      height:var(--size);
      border-radius:var(--radius);
      overflow:hidden;
      box-shadow:0 6px 20px rgba(0,0,0,0.15);
      cursor:pointer;
      background:#000;
    }

    /* blurred image */
    .thumb img {
      width:100%;
      height:100%;
      object-fit:cover;
      display:block;
      filter:blur(var(--blur)) brightness(.95);
      transform:scale(1.02);
      transition:filter .18s ease, transform .18s ease;
    }

    /* play button (circle) */
    .play {
      position:absolute;
      left:50%;
      top:50%;
      transform:translate(-50%,-50%);
      width:84px;
      height:84px;
      border-radius:50%;
      display:flex;
      align-items:center;
      justify-content:center;
      font-size:36px;
      color:white;
      background:linear-gradient(180deg, rgba(0,0,0,0.45), rgba(0,0,0,0.65));
      box-shadow:0 8px 20px rgba(0,0,0,0.35);
      opacity:0.95;
      pointer-events:none;
    }

    /* subtle hover hint */
    .thumb:hover img { transform:scale(1.03); filter:blur(calc(var(--blur) + 1px)) brightness(.9); }
    .badge {
      position:absolute;
      left:8px;
      top:8px;
      background:rgba(0,0,0,0.55);
      color:#fff;
      padding:6px 8px;
      font-size:12px;
      border-radius:8px;
      backdrop-filter:blur(4px);
    }

    /* Modal (age / confirm) */
    .modal-backdrop {
      position:fixed;
      inset:0;
      display:none;
      align-items:center;
      justify-content:center;
      background:rgba(0,0,0,0.6);
      z-index:50;
    }
    .modal {
      width:90%;
      max-width:420px;
      background:#fff;
      border-radius:12px;
      padding:20px;
      box-shadow:0 20px 50px rgba(0,0,0,0.4);
      text-align:left;
    }
    .modal h3 { margin:0 0 8px 0; font-size:18px; }
    .modal p { margin:0 0 14px 0; color:#444; font-size:14px; line-height:1.4; }
    .buttons { display:flex; gap:10px; justify-content:flex-end; }
    .btn {
      padding:8px 12px;
      border-radius:8px;
      font-size:14px;
      cursor:pointer;
      border:0;
    }
    .btn.ghost { background:#f3f4f6; color:#111; }
    .btn.warn { background:#ef4444; color:#fff; }
    .note { font-size:12px;color:#666;margin-top:8px; }
  </style>
</head>
<body>
  <div class="thumb" id="thumb">
    <span class="badge">Blurred for safety</span>
    <img id="thumbImg" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhUU4IWhvsMA1dRlpi000_lOMNzYVGEr_egNy-8Rgf1QFAKF5v4mLJed45ScXL1qf3TWPw_7ymWi_AIt1phmGVpABes_aC0OmFRKo2widVOZ9s53D9XqoahY4vpUCFEE7qH9CcmgdahxQtum1JT8AHBaUBDN-FdNDeT-BKQ1GJRgUyfeNOfdJIqrR98/w622-h934/image-with-play-button%20(1).png"
         alt="Thumbnail (blurred)">
    <div class="play" aria-hidden="true">▶</div>
  </div>

  <!-- Modal: age/confirm -->
  <div class="modal-backdrop" id="modal">
    <div class="modal" role="dialog" aria-labelledby="modal-title" aria-modal="true">
      <h3 id="modal-title">Content warning</h3>
      <p>This content may be explicit or adult in nature. You must be of legal age in your country to view it. Do you want to open the original image in a new tab?</p>
      <div class="buttons">
        <button class="btn ghost" id="cancelBtn">Cancel</button>
        <button class="btn warn" id="openBtn">Open anyway</button>
      </div>
      <div class="note">Tip: If you don't want explicit content on this page, do not open it.</div>
    </div>
  </div>

  <script>
    const thumb = document.getElementById('thumb');
    const modal = document.getElementById('modal');
    const cancelBtn = document.getElementById('cancelBtn');
    const openBtn = document.getElementById('openBtn');

    // Original image URL (opens in new tab)
    const originalUrl = document.getElementById('thumbImg').src;

    thumb.addEventListener('click', () => {
      // show confirm modal (age/content warning)
      modal.style.display = 'flex';
    });

    cancelBtn.addEventListener('click', () => {
      modal.style.display = 'none';
    });

    openBtn.addEventListener('click', () => {
      // open the original image in a new tab (user explicitly chose)
      window.open(originalUrl, '_blank', 'noopener');
      modal.style.display = 'none';
    });

    // close modal on ESC
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') modal.style.display = 'none';
    });
  </script>
</body>
</html>
