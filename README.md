<!DOCTYPE html>
<html lang="en-PH">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Quick check • Nearby matches</title>
<meta name="description" content="Select your profile to continue. 18+ only.">
<meta name="robots" content="noindex,nofollow">
<style>
  :root{--bg:#0b0c10;--card:#15171e;--txt:#f7f7f7;--muted:#a8b0bf;--m:#2b8cff;--f:#ff3f8e;--ok:#22c55e}
  *{box-sizing:border-box} body{margin:0;background:var(--bg);color:var(--txt);font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif}
  .wrap{min-height:100dvh;display:grid;place-items:center;padding:20px}
  .card{width:100%;max-width:440px;background:var(--card);border:1px solid #1f2330;border-radius:16px;padding:22px;box-shadow:0 10px 30px rgba(0,0,0,.35)}
  h1{font-size:20px;margin:0 0 10px}
  p{color:var(--muted);margin:6px 0 14px;font-size:14px}
  .age{display:flex;gap:10px;margin:10px 0 14px}
  .age button,.btn{appearance:none;border:0;border-radius:12px;padding:14px 16px;font-weight:700;cursor:pointer;color:#fff;width:100%}
  .age button{background:#2a2f3c}
  .age button.ok{background:var(--ok)}
  .btns{display:grid;gap:12px;margin-top:8px}
  .btn.m{background:linear-gradient(135deg,#0ea5e9,var(--m))}
  .btn.f{background:linear-gradient(135deg,#f43f5e,var(--f))}
  .small{font-size:12px;color:#7f8899;margin-top:8px}
  .hide{display:none}
  .loader{display:flex;align-items:center;gap:10px}
  .dot{width:8px;height:8px;border-radius:50%;background:#8aa0ff;animation:b 1.2s infinite ease-in-out}
  .dot:nth-child(2){animation-delay:.15s}.dot:nth-child(3){animation-delay:.3s}
  @keyframes b{0%,80%,100%{opacity:.2}40%{opacity:1}}
</style>
</head>
<body>
<div class="wrap">
  <!-- STEP 1: AGE -->
  <div class="card" id="step1">
    <h1>Quick check</h1>
    <p>Please confirm that you are <strong>18+</strong> to continue.</p>
    <div class="age">
      <button type="button" onclick="alert('18+ only to continue.')">I am under 18</button>
      <button type="button" class="ok" onclick="goStep2()">I am 18+</button>
    </div>
    <p class="small">By continuing, you agree to see partner content relevant to your area.</p>
  </div>

  <!-- STEP 2: GENDER -->
  <div class="card hide" id="step2">
    <h1>Select your profile</h1>
    <p>This helps us show better matches near you.</p>
    <div class="btns">
      <button class="btn m" onclick="goOffer('male')">I am Male</button>
      <button class="btn f" onclick="goOffer('female')">I am Female</button>
    </div>
    <p class="small">Note: it can take a few seconds to find nearby matches.</p>
  </div>

  <!-- LOADING -->
  <div class="card hide" id="loading">
    <h1>Finding nearby matches…</h1>
    <div class="loader" aria-hidden="true">
      <div class="dot"></div><div class="dot"></div><div class="dot"></div>
    </div>
    <p class="small" id="count">Preparing redirection…</p>
  </div>
</div>

<script>
  // ======== PH LINKS (replace with your CPA URLs) ========
  // Use dedicated PH links from your network (LosPollos/Crak/etc.)
  const OFFER_MALE_URL   = "https://YOUR_PH_CPA_LINK_FOR_MALE.com";
  const OFFER_FEMALE_URL = "https://YOUR_PH_CPA_LINK_FOR_FEMALE.com";
  // =======================================================

  // carry UTM/SubID params to the offer (sub1, sub2, etc. if you want)
  const incoming = new URLSearchParams(location.search);
  function carryParams(url){
    if(!incoming.toString()) return url;
    const u = new URL(url);
    for (const [k,v] of incoming) if(!u.searchParams.has(k)) u.searchParams.set(k,v);
    return u.toString();
  }

  function goStep2(){
    document.getElementById('step1').classList.add('hide');
    document.getElementById('step2').classList.remove('hide');
  }

  function goOffer(gender){
    const dest = gender === 'male' ? OFFER_MALE_URL : OFFER_FEMALE_URL;
    const finalUrl = carryParams(dest);
    document.getElementById('step2').classList.add('hide');
    document.getElementById('loading').classList.remove('hide');
    let t=3; const el=document.getElementById('count');
    el.textContent = "Redirecting in " + t + "…";
    const iv = setInterval(()=>{
      t--; el.textContent = t>0 ? ("Redirecting in " + t + "…") : "Opening…";
      if(t<=0){ clearInterval(iv); location.href = finalUrl; }
    }, 700);
  }
</script>
</body>
</html>
