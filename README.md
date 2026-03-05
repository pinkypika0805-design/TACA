<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>製程計算與換算工具</title>
    <style>
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        body { font-family: -apple-system, system-ui, sans-serif; background: #f4f7f9; margin: 0; padding: 15px; }
        .card { background: white; border-radius: 16px; padding: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.08); margin-bottom: 20px; max-width: 500px; margin-left: auto; margin-right: auto; }
        h2 { font-size: 1.2rem; margin-top: 0; color: #2c3e50; border-left: 5px solid #3498db; padding-left: 10px; }
        .input-row { display: flex; gap: 10px; margin-bottom: 12px; }
        .input-group { flex: 1; }
        label { display: block; font-size: 0.85rem; color: #666; margin-bottom: 4px; font-weight: bold; }
        input { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 8px; font-size: 16px; background: #fafafa; }
        .result-box { background: #eef7fe; border-radius: 10px; padding: 15px; margin-top: 15px; text-align: center; }
        .res-label { font-size: 0.9rem; color: #5d6d7e; }
        .res-val { font-size: 1.5rem; font-weight: 900; color: #2980b9; display: block; margin: 5px 0; }
        .divider { border-top: 1px dashed #ddd; margin: 20px 0; }
        .tool-row { display: flex; align-items: center; gap: 10px; margin-top: 10px; }
    </style>
</head>
<body>

<div class="card">
    <h2>⚙️ 製程計算 (主程式)</h2>
    <label>1. 本製程 (現況)</label>
    <div class="input-row">
        <div class="input-group"><label>寬</label><input type="number" id="w1" oninput="calcMain()"></div>
        <div class="input-group"><label>厚</label><input type="number" id="t1" oninput="calcMain()"></div>
        <div class="input-group"><label>速</label><input type="number" id="s1" oninput="calcMain()"></div>
    </div>
    <div class="result-box" style="background:#f9f9f9;">
        <span class="res-label">目前總流量 (M)</span>
        <span id="m_now" class="res-val" style="color:#7f8c8d;">---</span>
    </div>
    <div class="divider"></div>
    <label>2. 下一製程 (目標規格)</label>
    <div class="input-row">
        <div class="input-group"><label>目標寬</label><input type="number" id="w2" oninput="calcMain()"></div>
        <div class="input-group"><label>目標厚</label><input type="number" id="t2" oninput="calcMain()"></div>
    </div>
    <div class="result-box">
        <span class="res-label">建議 料速 (S)</span>
        <span id="s_suggest" class="res-val">---</span>
    </div>
</div>

<div class="card">
    <h2>🔄 快速換算工具</h2>
    <div class="input-row">
        <div class="input-group"><label>基準 料量 (M)</label><input type="number" id="bm" oninput="calcSwap()"></div>
        <div class="input-group"><label>基準 料速 (S)</label><input type="number" id="bs" oninput="calcSwap()"></div>
    </div>
    <div class="divider"></div>
    <label>👉 改料量求料速：</label>
    <div class="tool-row">
        <input type="number" id="nm" placeholder="新料量" oninput="calcSwap()">
        <span style="color:#95a5a6;">➔</span>
        <div style="flex:1; text-align:right;"><span id="rs" class="res-val" style="font-size:1.2rem;">---</span></div>
    </div>
    <label style="margin-top:15px; display:block;">👉 改料速求料量：</label>
    <div class="tool-row">
        <input type="number" id="ns" placeholder="新料速" oninput="calcSwap()">
        <span style="color:#95a5a6;">➔</span>
        <div style="flex:1; text-align:right;"><span id="rm" class="res-val" style="font-size:1.2rem;">---</span></div>
    </div>
</div>

<script>
    function calcMain() {
        const w1 = parseFloat(document.getElementById('w1').value) || 0;
        const t1 = parseFloat(document.getElementById('t1').value) || 0;
        const s1 = parseFloat(document.getElementById('s1').value) || 0;
        const w2 = parseFloat(document.getElementById('w2').value) || 0;
        const t2 = parseFloat(document.getElementById('t2').value) || 0;

        const mNow = w1 * t1 * s1;
        document.getElementById('m_now').innerText = mNow ? mNow.toFixed(2) : "---";

        if (mNow && w2 && t2) {
            const sSuggest = mNow / (w2 * t2);
            document.getElementById('s_suggest').innerText = sSuggest.toFixed(2);
        } else {
            document.getElementById('s_suggest').innerText = "---";
        }
    }

    function calcSwap() {
        const bm = parseFloat(document.getElementById('bm').value) || 0;
        const bs = parseFloat(document.getElementById('bs').value) || 0;
        const nm = parseFloat(document.getElementById('nm').value) || 0;
        const ns = parseFloat(document.getElementById('ns').value) || 0;
        const total = bm * bs;

        if (total && nm) document.getElementById('rs').innerText = (total / nm).toFixed(2);
        if (total && ns) document.getElementById('rm').innerText = (total / ns).toFixed(2);
    }
</script>
</body>
</html>
