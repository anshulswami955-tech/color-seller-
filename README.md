<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Color Seller — Fresh Finds, Best Price</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>

<style>
  /* ============================================================
     COLOR SELLER — single page shop
     ============================================================
     SETUP CHECKLIST BEFORE GOING LIVE:
     1) EmailJS (free): create account at emailjs.com
        - Add an Email Service  -> copy the Service ID
        - Create an Email Template with variables:
            {{customer_name}} {{customer_phone}} {{order_items}}
            {{order_total}} {{to_email}}
          -> copy the Template ID
        - Account > General -> copy your Public Key
        Paste all three into the CONFIG block at the top of the
        <script> section near the bottom of this file.
     2) Replace HERO_IMAGE / product image URLs with your own if
        you'd like — search for CONFIG.products below.
     3) WhatsApp number is used as given: wa.me/1235585279 — add a
        country code in front of it if it's missing one.
     ============================================================ */

  :root{
    --ink:#16151a;
    --ink-soft:#221f28;
    --paper:#f5f2ec;
    --coral:#ff6b4a;
    --gold:#ffc24b;
    --teal:#2bb3a3;
    --violet:#8b6bff;
    --lime:#c8f169;
    --muted:#8a8792;
    --radius:18px;
    --shadow:0 18px 40px -18px rgba(0,0,0,.35);
  }

  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--paper);
    color:var(--ink);
    font-family:'Inter',system-ui,sans-serif;
    -webkit-font-smoothing:antialiased;
  }
  h1,h2,h3,.display{
    font-family:'Space Grotesk',system-ui,sans-serif;
    letter-spacing:-0.02em;
    margin:0;
  }
  img{max-width:100%;display:block;}
  a{color:inherit;}
  button{font-family:inherit;cursor:pointer;border:none;}

  @media (prefers-reduced-motion: reduce){
    *{animation-duration:.01ms !important; animation-iteration-count:1 !important; transition-duration:.01ms !important; scroll-behavior:auto !important;}
  }

  :focus-visible{outline:3px solid var(--violet); outline-offset:3px;}

  /* ---------- topbar ---------- */
  .topbar{
    position:fixed; top:0; left:0; right:0; z-index:40;
    display:flex; align-items:center; justify-content:space-between;
    padding:16px 20px;
    background:linear-gradient(180deg, rgba(22,21,26,.85), rgba(22,21,26,0));
    pointer-events:none;
  }
  .topbar > *{pointer-events:auto;}
  .brand{
    display:flex; align-items:baseline; gap:8px;
    color:var(--paper); font-weight:700; font-size:1.15rem;
  }
  .brand .dot{color:var(--coral);}

  /* ---------- hero ---------- */
  .hero{
    position:relative;
    min-height:100svh;
    display:flex; align-items:flex-end;
    overflow:hidden;
    background:var(--ink);
  }
  .hero-img{
    position:absolute; inset:0; width:100%; height:100%;
    object-fit:cover; object-position:center 30%;
    opacity:.55;
  }
  .hero::before{
    content:"";
    position:absolute; inset:0;
    background:
      radial-gradient(60% 50% at 15% 15%, rgba(255,107,74,.35), transparent 60%),
      radial-gradient(55% 45% at 85% 10%, rgba(139,107,255,.30), transparent 60%),
      linear-gradient(180deg, rgba(22,21,26,.35) 0%, rgba(22,21,26,.55) 55%, rgba(22,21,26,.95) 100%);
  }
  .hero-content{
    position:relative; z-index:2;
    padding:0 20px 88px;
    max-width:720px;
    color:var(--paper);
  }
  .eyebrow{
    display:inline-flex; align-items:center; gap:8px;
    font-size:.8rem; font-weight:600; letter-spacing:.14em; text-transform:uppercase;
    color:var(--gold); margin-bottom:18px;
  }
  .eyebrow .swatch{width:9px;height:9px;border-radius:50%;background:var(--coral);}
  .hero h1{
    font-size:clamp(2.6rem, 9vw, 5.2rem);
    line-height:.98;
    font-weight:700;
  }
  .hero h1 span{color:var(--coral);}
  .hero p{
    margin-top:18px;
    font-size:clamp(1rem,2.4vw,1.25rem);
    color:#d8d5cf;
    max-width:32ch;
  }
  .cta-row{margin-top:32px; display:flex; gap:14px; flex-wrap:wrap;}
  .btn{
    display:inline-flex; align-items:center; gap:10px;
    padding:15px 26px; border-radius:100px;
    font-weight:600; font-size:.98rem;
    transition:transform .18s ease, box-shadow .18s ease;
  }
  .btn:active{transform:scale(.96);}
  .btn-primary{
    background:var(--coral); color:var(--ink);
    box-shadow:0 10px 26px -8px rgba(255,107,74,.6);
  }
  .btn-primary:hover{transform:translateY(-2px);}
  .btn-ghost{
    background:rgba(245,242,236,.08);
    color:var(--paper);
    border:1px solid rgba(245,242,236,.35);
  }
  .btn-ghost:hover{background:rgba(245,242,236,.16);}

  /* ---------- paint drip divider (signature element) ---------- */
  .drip{
    display:block; width:100%; height:64px;
    margin-top:-1px;
  }
  .drip path{fill:var(--paper);}

  /* ---------- products ---------- */
  .section{padding:64px 0 88px;}
  .section-head{
    padding:0 20px; margin-bottom:34px;
    display:flex; align-items:flex-end; justify-content:space-between; gap:16px; flex-wrap:wrap;
  }
  .section-head h2{font-size:clamp(1.7rem,4vw,2.4rem); font-weight:700;}
  .section-head p{color:var(--muted); margin:6px 0 0; font-size:.95rem;}

  .carousel{
    display:flex; gap:18px;
    overflow-x:auto;
    padding:6px 20px 20px;
    scroll-snap-type:x mandatory;
    scrollbar-width:thin;
    scrollbar-color:var(--ink) transparent;
  }
  .carousel::-webkit-scrollbar{height:6px;}
  .carousel::-webkit-scrollbar-thumb{background:#d8d3c7; border-radius:10px;}

  .card{
    flex:0 0 auto;
    width:min(76vw,270px);
    scroll-snap-align:start;
    background:#fff;
    border-radius:var(--radius);
    overflow:hidden;
    box-shadow:var(--shadow);
    display:flex; flex-direction:column;
    position:relative;
  }
  .card-swatch{
    position:absolute; top:14px; left:14px; z-index:2;
    width:22px; height:22px; border-radius:50%;
    border:3px solid #fff; box-shadow:0 2px 6px rgba(0,0,0,.25);
  }
  .card-media{aspect-ratio:4/5; overflow:hidden; background:#eee;}
  .card-media img{width:100%; height:100%; object-fit:cover; transition:transform .5s ease;}
  .card:hover .card-media img{transform:scale(1.06);}
  .card-body{padding:16px 16px 18px; display:flex; flex-direction:column; gap:10px; flex:1;}
  .card-body h3{font-size:1.05rem; font-weight:600;}
  .price-row{display:flex; align-items:center; justify-content:space-between; margin-top:auto;}
  .price{font-family:'Space Grotesk',sans-serif; font-weight:700; font-size:1.15rem;}
  .add-btn{
    padding:10px 16px; border-radius:100px;
    background:var(--ink); color:var(--paper);
    font-size:.85rem; font-weight:600;
    display:inline-flex; align-items:center; gap:6px;
    transition:transform .15s ease, background .15s ease;
  }
  .add-btn:hover{background:var(--coral); color:var(--ink);}
  .add-btn:active{transform:scale(.94);}

  /* ---------- info strip ---------- */
  .strip{
    background:var(--ink); color:var(--paper);
    padding:54px 20px; text-align:center;
  }
  .strip h2{font-size:clamp(1.5rem,4vw,2.1rem); max-width:26ch; margin:0 auto;}
  .strip p{color:#b9b6c0; margin-top:12px; font-size:.95rem;}

  /* ---------- footer ---------- */
  footer{padding:40px 20px 120px; text-align:center; color:var(--muted); font-size:.85rem;}
  footer a{text-decoration:underline;}

  /* ---------- floating cart ---------- */
  .fab-cart{
    position:fixed; right:20px; bottom:22px; z-index:50;
    width:60px; height:60px; border-radius:50%;
    background:var(--coral); color:var(--ink);
    display:flex; align-items:center; justify-content:center;
    box-shadow:0 14px 30px -10px rgba(255,107,74,.7);
    transition:transform .18s ease;
  }
  .fab-cart:hover{transform:translateY(-3px) scale(1.04);}
  .fab-cart .count{
    position:absolute; top:-4px; right:-4px;
    min-width:22px; height:22px; padding:0 5px;
    border-radius:50%; background:var(--ink); color:var(--paper);
    font-size:.72rem; font-weight:700;
    display:flex; align-items:center; justify-content:center;
    border:2px solid var(--paper);
  }

  /* ---------- floating whatsapp ---------- */
  .fab-wa{
    position:fixed; left:20px; bottom:22px; z-index:50;
    width:56px; height:56px; border-radius:50%;
    background:#25D366; color:#fff;
    display:flex; align-items:center; justify-content:center;
    box-shadow:0 14px 30px -10px rgba(37,211,102,.8);
    transition:transform .18s ease;
  }
  .fab-wa:hover{transform:translateY(-3px) scale(1.04);}

  /* ---------- cart drawer ---------- */
  .overlay{
    position:fixed; inset:0; background:rgba(22,21,26,.55);
    opacity:0; pointer-events:none; transition:opacity .25s ease;
    z-index:60;
  }
  .overlay.open{opacity:1; pointer-events:auto;}

  .drawer{
    position:fixed; top:0; right:0; bottom:0; z-index:61;
    width:min(420px,100%);
    background:var(--paper);
    transform:translateX(100%);
    transition:transform .3s cubic-bezier(.2,.8,.2,1);
    display:flex; flex-direction:column;
    box-shadow:-20px 0 50px -20px rgba(0,0,0,.4);
  }
  .drawer.open{transform:translateX(0);}
  .drawer-head{
    padding:20px; display:flex; align-items:center; justify-content:space-between;
    border-bottom:1px solid #e4e0d6;
  }
  .drawer-head h2{font-size:1.3rem;}
  .icon-btn{
    width:38px; height:38px; border-radius:50%;
    background:#eeeae0; display:flex; align-items:center; justify-content:center;
  }
  .drawer-body{flex:1; overflow-y:auto; padding:16px 20px;}
  .empty-msg{color:var(--muted); text-align:center; padding:50px 10px; font-size:.95rem;}

  .cart-item{
    display:flex; gap:12px; padding:12px 0; border-bottom:1px solid #ece8de;
  }
  .cart-item img{width:64px; height:64px; border-radius:12px; object-fit:cover; flex:0 0 auto;}
  .cart-item-info{flex:1; min-width:0;}
  .cart-item-info h4{font-size:.95rem; margin:0 0 4px; font-weight:600;}
  .qty-row{display:flex; align-items:center; gap:10px; margin-top:6px;}
  .qty-btn{
    width:26px; height:26px; border-radius:50%; background:#eeeae0;
    display:flex; align-items:center; justify-content:center; font-weight:700;
  }
  .qty-val{font-weight:600; min-width:16px; text-align:center; font-size:.9rem;}
  .remove-link{margin-left:auto; font-size:.78rem; color:var(--muted); text-decoration:underline;}
  .item-price{font-weight:700; font-family:'Space Grotesk',sans-serif; white-space:nowrap;}

  .drawer-footer{
    padding:18px 20px 22px; border-top:1px solid #e4e0d6; background:#fff;
  }
  .total-row{display:flex; justify-content:space-between; align-items:center; margin-bottom:14px;}
  .total-row .label{color:var(--muted); font-size:.9rem;}
  .total-row .value{font-size:1.35rem; font-weight:700; font-family:'Space Grotesk',sans-serif;}

  .field{margin-bottom:12px;}
  .field label{display:block; font-size:.8rem; font-weight:600; margin-bottom:6px; color:var(--ink-soft);}
  .field input{
    width:100%; padding:12px 14px; border-radius:10px;
    border:1.5px solid #ddd7c8; background:#faf8f2; font-size:.95rem; font-family:inherit;
  }
  .field input:focus{border-color:var(--violet); background:#fff;}

  .send-btn{
    width:100%; padding:15px; border-radius:100px;
    background:var(--ink); color:var(--paper); font-weight:700; font-size:1rem;
    display:flex; align-items:center; justify-content:center; gap:8px;
    transition:opacity .2s ease, transform .15s ease;
  }
  .send-btn:active{transform:scale(.98);}
  .send-btn:disabled{opacity:.6; cursor:not-allowed;}
  .form-note{font-size:.75rem; color:var(--muted); margin-top:10px; text-align:center;}

  /* ---------- toast ---------- */
  .toast{
    position:fixed; left:50%; bottom:96px; z-index:80;
    transform:translate(-50%,20px);
    background:var(--ink); color:var(--paper);
    padding:12px 20px; border-radius:100px; font-size:.9rem; font-weight:600;
    opacity:0; pointer-events:none; transition:all .25s ease;
    box-shadow:var(--shadow);
    max-width:90vw; text-align:center;
  }
  .toast.show{opacity:1; transform:translate(-50%,0);}

  @media (min-width:720px){
    .card{width:250px;}
  }
</style>
</head>
<body>

  <div class="topbar">
    <div class="brand">Color<span class="dot">•</span>Seller</div>
  </div>

  <!-- ================= HERO ================= -->
  <section class="hero" id="top">
    <img class="hero-img" id="heroImg" src="" alt="">
    <div class="hero-content">
      <div class="eyebrow"><span class="swatch"></span> Fresh Finds, Best Price</div>
      <h1>Every shade<br>you've been<br>chasing<span>.</span></h1>
      <p>Hand-picked colors, honest prices, and a cart that goes straight to us on WhatsApp or email.</p>
      <div class="cta-row">
        <button class="btn btn-primary" onclick="document.getElementById('products').scrollIntoView({behavior:'smooth'})">
          Shop Now
        </button>
        <a class="btn btn-ghost" href="#products" onclick="event.preventDefault();document.getElementById('products').scrollIntoView({behavior:'smooth'})">See what's new</a>
      </div>
    </div>
    <svg class="drip" viewBox="0 0 1440 64" preserveAspectRatio="none" aria-hidden="true">
      <path d="M0,20 C120,60 240,0 360,24 C480,48 600,4 720,20 C840,36 960,4 1080,20 C1200,36 1320,54 1440,22 L1440,64 L0,64 Z"></path>
    </svg>
  </section>

  <!-- ================= PRODUCTS ================= -->
  <section class="section" id="products">
    <div class="section-head">
      <div>
        <h2>Today's picks</h2>
        <p>Swipe through — new colors added weekly.</p>
      </div>
    </div>
    <div class="carousel" id="carousel"></div>
  </section>

  <div class="strip">
    <h2>Add to cart, tell us who you are, and we'll take it from there.</h2>
    <p>No accounts, no checkout forms — just a quick order sent straight to the shop.</p>
  </div>

  <footer>
    Color Seller — Fresh Finds, Best Price · Order questions?
    <a href="mailto:Swamiji050505@gmail.com">Swamiji050505@gmail.com</a>
  </footer>

  <!-- ================= FLOATING BUTTONS ================= -->
  <button class="fab-cart" id="cartFab" aria-label="Open cart">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="21" r="1"></circle><circle cx="20" cy="21" r="1"></circle><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>
    <span class="count" id="cartCount">0</span>
  </button>

  <a class="fab-wa" id="waFab" href="https://wa.me/1235585279" target="_blank" rel="noopener" aria-label="Chat on WhatsApp">
    <svg width="26" height="26" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.626.712.226 1.36.194 1.872.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347z"></path><path d="M12.001 2C6.478 2 2 6.478 2 12c0 1.876.52 3.63 1.42 5.13L2 22l4.99-1.377A9.953 9.953 0 0 0 12.001 22C17.523 22 22 17.523 22 12S17.523 2 12.001 2zm0 18.2a8.166 8.166 0 0 1-4.166-1.14l-.298-.177-2.96.816.79-2.884-.194-.297A8.184 8.184 0 1 1 20.2 12a8.209 8.209 0 0 1-8.199 8.2z"></path></svg>
  </a>

  <!-- ================= CART DRAWER ================= -->
  <div class="overlay" id="overlay"></div>
  <aside class="drawer" id="drawer" aria-label="Shopping cart">
    <div class="drawer-head">
      <h2>Your cart</h2>
      <button class="icon-btn" id="closeDrawer" aria-label="Close cart">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
      </button>
    </div>
    <div class="drawer-body" id="cartItems"></div>
    <div class="drawer-footer">
      <div class="total-row">
        <span class="label">Total</span>
        <span class="value" id="cartTotal">₹0</span>
      </div>
      <div class="field">
        <label for="custName">Your name</label>
        <input id="custName" type="text" placeholder="e.g. Priya Sharma" autocomplete="name">
      </div>
      <div class="field">
        <label for="custPhone">Phone number</label>
        <input id="custPhone" type="tel" placeholder="e.g. 98765 43210" autocomplete="tel">
      </div>
      <button class="send-btn" id="sendOrder">Send Order</button>
      <p class="form-note">We'll email your order to the shop and get back to you on this number.</p>
    </div>
  </aside>

  <div class="toast" id="toast"></div>

<script>
  /* ===================== CONFIG ===================== */
  const CONFIG = {
    // --- EmailJS: paste your own IDs here (see checklist at top of file) ---
    emailjsPublicKey:  "YOUR_EMAILJS_PUBLIC_KEY",
    emailjsServiceId:  "YOUR_EMAILJS_SERVICE_ID",
    emailjsTemplateId: "YOUR_EMAILJS_TEMPLATE_ID",
    ownerEmail: "Swamiji050505@gmail.com",
    currency: "₹",

    heroImage: "https://i.ibb.co/cK9bdZY3/IMG20260808081153.jpg",

    products: [
      { id:"p1", name:"Sunset Coral Palette", price:499, img:"https://i.ibb.co/cK9bdZY3/IMG20260808081153.jpg", swatch:"#ff6b4a" },
      { id:"p2", name:"Ocean Teal Set",       price:599, img:"https://i.ibb.co/hRxC00N2/IMG20260808081444.jpg", swatch:"#2bb3a3" },
      { id:"p3", name:"Golden Hour Mix",      price:449, img:"https://i.ibb.co/kg8FqJkn/IMG20260808081503.jpg", swatch:"#ffc24b" },
      { id:"p4", name:"Violet Dream Pack",    price:549, img:"https://i.ibb.co/ksF2PYM6/IMG20260808081357.jpg", swatch:"#8b6bff" },
      { id:"p5", name:"Meadow Lime Combo",    price:399, img:"https://i.ibb.co/1tkyBKNg/IMG20260808081334.jpg", swatch:"#c8f169" },
    ]
  };

  document.getElementById('heroImg').src = CONFIG.heroImage;

  if (window.emailjs && CONFIG.emailjsPublicKey && !CONFIG.emailjsPublicKey.startsWith("YOUR_")) {
    emailjs.init({ publicKey: CONFIG.emailjsPublicKey });
  }

  /* ===================== RENDER PRODUCTS ===================== */
  const carousel = document.getElementById('carousel');
  CONFIG.products.forEach(p => {
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <span class="card-swatch" style="background:${p.swatch}"></span>
      <div class="card-media"><img src="${p.img}" alt="${p.name}" loading="lazy"></div>
      <div class="card-body">
        <h3>${p.name}</h3>
        <div class="price-row">
          <span class="price">${CONFIG.currency}${p.price}</span>
          <button class="add-btn" data-id="${p.id}">Add to cart</button>
        </div>
      </div>`;
    carousel.appendChild(card);
  });

  /* ===================== CART STATE ===================== */
  let cart = {}; // id -> qty

  function findProduct(id){ return CONFIG.products.find(p => p.id === id); }

  function addToCart(id){
    cart[id] = (cart[id] || 0) + 1;
    renderCart();
    showToast(findProduct(id).name + " added to cart");
  }
  function changeQty(id, delta){
    cart[id] = (cart[id] || 0) + delta;
    if (cart[id] <= 0) delete cart[id];
    renderCart();
  }
  function removeItem(id){
    delete cart[id];
    renderCart();
  }
  function cartTotal(){
    return
