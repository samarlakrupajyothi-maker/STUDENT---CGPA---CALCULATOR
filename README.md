
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CGPA Calculator – SRK Institute of Technology</title>
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@500;600;700&family=Nunito:wght@400;600;700;800&display=swap" rel="stylesheet">
<!-- jsPDF for PDF download -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  :root {
    --navy: #1a2a5e;
    --blue: #1e3a8a;
    --cyan: #00c6e0;
    --cyan-dark: #009bb5;
    --orange: #e85d04;
    --white: #ffffff;
    --card-bg: rgba(255,255,255,0.07);
    --input-bg: rgba(255,255,255,0.13);
    --border: rgba(0,198,224,0.35);
    --shadow: 0 8px 40px rgba(0,0,0,0.35);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    min-height: 100vh;
    background: linear-gradient(135deg, #0d1b4b 0%, #1a2a5e 40%, #0a3d62 100%);
    font-family: 'Nunito', sans-serif;
    color: var(--white);
    display: flex; flex-direction: column; align-items: center;
  }
  header {
    width: 100%;
    background: linear-gradient(90deg, #fff 0%, #f0f4ff 100%);
    padding: 14px 24px;
    display: flex; align-items: center; gap: 16px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  }
  .logo-wrap {
    flex-shrink: 0; width: 72px; height: 72px;
    border-radius: 50%; border: 3px solid var(--orange);
    background: #fff; display: flex; align-items: center;
    justify-content: center; overflow: hidden;
  }
  .logo-wrap img { width: 100%; height: 100%; object-fit: contain; }
  .header-text { flex: 1; }
  .header-text h1 {
    font-family: 'Rajdhani', sans-serif;
    font-size: clamp(17px,3vw,24px); color: var(--navy);
    font-weight: 700; letter-spacing: 0.5px; line-height: 1.1;
  }
  .header-text p {
    font-size: clamp(9px,1.5vw,12px); color: #e85d04;
    font-weight: 600; margin-top: 2px; letter-spacing: 0.3px;
  }
  main { width: min(540px,96vw); margin: 32px auto 40px; display: flex; flex-direction: column; gap: 24px; }
  .card {
    background: var(--card-bg); border: 1px solid var(--border);
    border-radius: 18px; padding: 28px 28px 24px;
    backdrop-filter: blur(12px); box-shadow: var(--shadow);
  }
  .card-title {
    font-family: 'Rajdhani', sans-serif; font-size: 20px; font-weight: 700;
    letter-spacing: 1px; color: var(--cyan); margin-bottom: 18px;
    display: flex; align-items: center; gap: 8px;
  }
  .card-title span { font-size: 22px; }
  label {
    display: block; font-size: 12px; font-weight: 700;
    color: rgba(255,255,255,0.7); text-transform: uppercase;
    letter-spacing: 0.8px; margin-bottom: 6px;
  }
  select, input[type="number"], input[type="text"] {
    width: 100%; padding: 11px 14px; border-radius: 10px;
    border: 1.5px solid var(--border); background: var(--input-bg);
    color: var(--white); font-family: 'Nunito', sans-serif;
    font-size: 15px; font-weight: 600; outline: none;
    transition: border-color .2s, background .2s;
  }
  select {
    appearance: none; -webkit-appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' fill='%2300c6e0' viewBox='0 0 16 16'%3E%3Cpath d='M7.247 11.14L2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z'/%3E%3C/svg%3E");
    background-repeat: no-repeat; background-position: right 12px center; padding-right: 36px;
  }
  select option { background: #1a2a5e; color: #fff; }
  input:focus, select:focus { border-color: var(--cyan); background: rgba(0,198,224,0.1); }
  input::placeholder { color: rgba(255,255,255,0.35); }
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  .sgpa-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 4px; }
  .sgpa-item label { margin-bottom: 4px; font-size: 11px; }
  .field-group { margin-bottom: 14px; }
  .btn {
    width: 100%; margin-top: 18px; padding: 13px; border: none;
    border-radius: 10px; font-family: 'Rajdhani', sans-serif;
    font-size: 17px; font-weight: 700; letter-spacing: 1.2px;
    cursor: pointer; transition: transform .15s, box-shadow .15s, opacity .15s;
  }
  .btn:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0,198,224,0.45); }
  .btn:active { transform: translateY(0); }
  .btn-primary { background: linear-gradient(90deg,var(--cyan),#0099c0); color: var(--navy); }
  .btn-orange  { background: linear-gradient(90deg,#e85d04,#f48c06); color: #fff; }
  .btn-green   { background: linear-gradient(90deg,#00c853,#00897b); color: #fff; }
  .result-box {
    margin-top: 18px; padding: 16px; border-radius: 12px;
    background: rgba(0,198,224,0.12); border: 1.5px solid var(--cyan);
    text-align: center; display: none;
  }
  .result-box.show { display: block; animation: fadeIn .4s ease; }
  .result-value { font-family: 'Rajdhani', sans-serif; font-size: 38px; font-weight: 700; color: var(--cyan); letter-spacing: 2px; }
  .result-label { font-size: 13px; color: rgba(255,255,255,0.65); margin-top: 2px; font-weight: 600; }
  .result-grade { margin-top: 8px; display: inline-block; padding: 4px 16px; border-radius: 20px; font-size: 13px; font-weight: 700; letter-spacing: 0.5px; }
  .target-result {
    margin-top: 14px; padding: 13px; border-radius: 10px;
    background: rgba(232,93,4,0.12); border: 1.5px solid var(--orange);
    text-align: center; display: none;
  }
  .target-result.show { display: block; animation: fadeIn .4s ease; }
  .target-result p { font-size: 15px; font-weight: 700; color: #ffd166; line-height: 1.5; }
  .target-result span { color: var(--orange); font-size: 20px; font-family: 'Rajdhani', sans-serif; }
  .history-list { list-style: none; display: flex; flex-direction: column; gap: 8px; }
  .history-list li {
    display: flex; justify-content: space-between; align-items: center;
    padding: 9px 14px; border-radius: 8px;
    background: rgba(255,255,255,0.05); border: 1px solid var(--border); font-size: 13px;
  }
  .history-list li .h-sem { color: rgba(255,255,255,0.55); font-size: 11px; }
  .history-list li .h-cgpa { color: var(--cyan); font-family: 'Rajdhani', sans-serif; font-size: 18px; font-weight: 700; }
  .empty-history { color: rgba(255,255,255,0.35); font-size: 13px; text-align: center; padding: 12px 0; }
  .btn-clear {
    width: auto; margin-top: 14px; padding: 8px 20px; font-size: 13px;
    border-radius: 8px; background: rgba(255,255,255,0.08);
    color: rgba(255,255,255,0.55); border: 1px solid rgba(255,255,255,0.15);
    cursor: pointer; transition: background .2s;
  }
  .btn-clear:hover { background: rgba(255,255,255,0.14); }
  @keyframes fadeIn { from { opacity:0; transform:translateY(6px); } to { opacity:1; transform:none; } }
  footer { font-size: 11px; color: rgba(255,255,255,0.3); text-align: center; padding: 0 0 20px; letter-spacing: 0.3px; }
</style>
</head>
<body>

<header>
  <div class="logo-wrap">
    <img src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAYGBgYHBgcICAcKCwoLCg8ODAwODxYQERAREBYiFRkVFRkVIh4kHhweJB42KiYmKjY+NDI0PkxERExfWl98fKcBBgYGBgcGBwgIBwoLCgsKDw4MDA4PFhAREBEQFiIVGRUVGRUiHiQeHB4kHjYqJiYqNj40MjQ+TERETF9aX3x8p//CABEIAJQA1AMBIgACEQEDEQH/xAAxAAEAAwEBAQAAAAAAAAAAAAAAAgMEAQUGAQADAQEBAAAAAAAAAAAAAAAAAQIDBAX/2gAMAwEAAhADEAAAAvlAAAACQc56NWyxjF6NebV1z5nLK+Ru936HnNmOQJYAAAAAAGmu1VKOhGvvO901dsJ86NRruS67ClmybsXJXC/MoSimAAAAlEHps+numEykcqRdzFVi/R55yX6ffLspegzaNTsJqUJuBnySjw0EsAFzVLVPQladUo8wQ5wt9TF+du9LPoq51VBynZazwo/S+Tk42+Zt0V0ZNl50N9PJWZpzwcEtryKXqKL+2XO5x0XUfRczuy0eNSvzTnz3U1Qm89ltVT6Xp/L+j0RLzfrPnUWzw7tkOWmCVPLQYsA3XRl3xzBu8/F+r6PmLMFcbuO53NePoVd0I68VPpZL58MNOfXzva0+Nf25edvy25mjnXQvM5bVwUCeu/JLZ62Fc6cso5VvzwjZTojLm126PMll3+i8qZXpVYoClRor04Y7sTp57Ix7L2MM9VqqpIoWuZ6u0wz7dnn3VVlujnlOvLs0nE1Njm6FfFUNeLQ5szciTthWWtsqqxa409WnZVSednaA+KV8slaosVgsV9CazuhUTkgsUVrOhUuiFa+sILCKyuSxWRYrBYrACYAAXxNVdUM7Ek6ATLK7SSHAFJAEsAAP/8QAAv/aAAwDAQACAAMAAAAhAAAGaIKjfBAAAAAENdrwDChuPCAAAAahLXQNCRBLAAFYDMidNRQLTOJAXJUOPzTh0IIHgAAp9GDWT0RU9DhATLyFQLNWmtzKVz6wvzJzkKBP0LJMzzz0z8080y5zzxAAAgee+8i+BAAA/8QAAv/aAAwDAQACAAMAAAAQ8884U0Tfs388888425LQhCBV98888+SDJJOpITF+8875DHUMW+6IR758tDEY3Sm0oOLBN88r1F82OQ6HdR18YJwdG+90yBQLLzeG1dhhaLWYj0CDNNPPCFJJDBHNNP8APPAfgQfI33/PPP/EADQRAAIBAwIDBQUJAAMAAAAAAAECAwAEERIhBTFhEyJBUYEQFFNxkRUgIzJCUpKhwWKx0f/aAAgBAgEBPwD2qpZlUcycCpeCzJBrVwzjmo8uns4TGBYxkgd4sf7xUqaJHT9rEfSrDhz3epi2hB44zk1d2xtpjGWDbA5H334fdpCspiOkj1HzFW0E80gESEkEHoPZ7tb6y/YpqJyTpFAADAGBUlrbSZ1woSfHG9RxpGioigKOQri8Uwu3kKHQcBW8OVQ2887aYoy1MrIxVgQQcEH7isVYMDgg5FWtwtxAko8RuPI0qKoIVQN87DFTTwwrqkcKKl45CDiOJm6k4r7dm+Cn1NR8dQnEkJHVTmoLq3uBmKQHpyNMqupVgCDzBpVSNMKAqj0FX1wLi5eQDbkvyHthhknkEca5Y0nCL0uoaPSpO7ZBwKiiSKNY0GFUYFcQ4glqukYMhGw8upqKxnumWa6lKhuWeZ+XkKSK3hzot07rEEv3v7PKhdqGCmaMDUM4KcunT+6aO3nkIaGJo+8S6+AH/IU9hgiS0dgwJIjbZ9vKuHcT7Y9jNtJ4HlqogEEEZBGCKuuE3CzsIYyyHcb8ulT2lxb6e1TTq5bg+yzumtZxIFB2wR0qCZJ4kkTOG86uJ1gheVuSj6mrSPtWkvLgg7nQDyLf+Cry4SAl378rjZTsMbjJHryqa5eQgu5OCBjwXPShKCRy/Niobh1ZWRirYyCDVrem57jNpmwNJ8Gx/tXkBlQzoMTR7yY5HqOoqwuvebdXP5xs3zokAVxS/S4IijAKKc6vM9PbbxiKCJP2oBXG5GIghX9Rz/gprcQohJXsokz1yviPnVzM8rvI+SWO+PCp59PPfw8ifnRuZOlQzh8g+o86ikOQQ243GPCrUC5SGfURsdSjkTyNcOzbcRnt/A5x6bj2TJ2c0iftYj6ey14xYwwIjRgldi3d519vW3w3/qrniME95BOM6E05GR4HVV3xy1mtpI0yC67EkU5VzgFSeX5sf9VLbM7k9rFgDz5CjYyDTmSPflvzpLNlbV2seFO+9RsFx+KuMctQNcO4rb20BRstlsgjHI0/Erb7RW6yQu222eWKPH7MDJVwPSpeMcMlOmSEseoXNM8eo4YYzXYylSNJHPHLy5fKoI3V5CV2bl0oQSgbpnK7jPkmK7KQpuu+iQeHMnamjcMzBCfxAdvLTijC4T8pzpiG2P0nepUdtGFJ7uPDbcc6SNhC4PM6ttqMUmX7jbqvljajFIX1aCo7vd28zQhlCEaM90geq0yyYx2ZOHY+G+oGhFL3l0+B364GKRdKKPIV7xcfGf6mvebj4z/yNC4n+M/1NdldaipuW/MQpye8AurNRe9yLGVnfLSaAMmuyutbKLo7ANkkjuH9XpSxXJRH95fSzgczspOA1GK7VNTXDDCsWGT3cch60YbtZI1a4fvBsnJ2KjJFKlyzR6bl9DKW1ZOwXnkVJLdRyMjSvlTg9417zcfGf+Rr3m4+M/8AI128/wAV/qfuBm1We52hb/amYiOAgkE27EnrjGaVmMKEsSeylGaLv2sq6jp7AbZ22UUGY3Nxlicyw5+tWbu7HUxP4vic81arpmQ3AUkAKgGNsAnNXRJaMk5JiTJ9Puf/xAAxEQACAQMCBAQFAwUBAAAAAAABAhEAAwQSIQUxQVEQEyJxFCAjMkIVUmEGgYKRsST/2gAIAQMBAT8A+QXBPg/3UKZopTIn59SzE0SBz8JPfwk9/BCIokD5iIMeABNC2epryx3o2/5ogjxUQPEkCta0TJpVmiwGwFST1qP4qSOpoN0NMkbjwDiN6BB5eDCRREGKAkxRMQopRNWsZ35CJts6z+QXnH+qfh7qt06XgWVcbfl6dQ/xmsnDeyboO4RwhJ2lo3imXTSmNulMIPgixufEmSatjmamfc1jWw9xVDoCIKh+THtXBuDm/AQBFB1k/eqT+yefKl4BgKFE3CQrCZ/dzri3BXxgtxGlQfRcIlkMQCfbvWbitZM6GVeUuRLn9wA6UdpFNuoPgNwPD9Oz2eBjXpKBwNDTpP5e1HFvhQxRgu28GN9xQw8pZQ2LknppPfT/AN2ocM4gCxOJe9LaW9DbN2NYlu/jILl21fVDDD6AZSOhBf3rh3H7GLjJbPDuIl3dpbyp1N7yJMCh/WWC3m6cHOJtAm5FseiP3b7Vl/1Tj3rXkfp2eGvIfL+mJPUFd94rOteaHZcG+LjOFDGwyHUemzESfarmHlBiDYuAgkEFSCCvP/U0uDmNqtrj3SwDEgISQFME/wBqt4OVduC3bsuzldQVVJMRMxQwszy1uizc8ttUPpOk6RJg/wAV8LlbfQucgftNfqvD7dxHN9bhItlo1hTF1Tqjo0CSOVcUzMS/jYS2rstZYB+fr1Aer+0RV3i2A7DTlhWS6SjaCR68lnM+wANLxDDt5RjIhPi8F4lioW2pDhSfxWrOZjXLVmy2Sq/+R0lpgN5+sA/4irXE8V8qTfXSb+cw16wsXUASdO9cOy8az8WLl9FjJFwRqIdQjrCzv+XWsnPs3OKYt1NKpbNqXBaTAEkyf+Vb4hhlMZmyLUrfvSSbmsByYIA2jee9W8/CtYxsNfS88Xh5pDyJt21AHSDBFHi3D7mSj/FKh85brsQfwvajEDmw3q1kYXmC58baTzMawkeqUNpkYhoH8U3EsAmxeW+B9W3Nv1fYHbVqERyasu+b+VfukzruMagdqgdqgdqkdqMDpUjtUjtUr2qV7Vt2oAHpUDtUDtUD5O/vQ6+9dT7iug966D2NNQ6UPk//xAA6EAACAQIDBQQGCQQDAAAAAAABAgADERIhMQQQE0FRICIyYRQjMEJicQUzQ1KBkaGxwUCC0fAkcoP/2gAIAQEAAT8C9gBfsUBmdxFiRvsbXt/QoLsBFULpKtK+Y37Pz3VfGdyqWMVQBK4GH+gTxr895pITOGnQS24qp5Q0UiIF3bRoPapRy70eky7konU+zq08cII13LQJ1jKVPsFNiIN2Bb3t2sS9RMa/eEv2WUNrFRV5bq5FvY0Gyt2WrKIazmEk6nsCq45xa45wEHsGO2I39jTTCN7MFGceqWiqW0i7P15a2h2CoiFzTGQuRcE262h2QKudUCphxYLHT59Y+xYdnWqXbOni8BI/ODZjUQuEv3wnmSZW2J6Rs6lTy5xqbLFYrpEqht5FxHXCe1TpYxPR/iiUcJ13swURmLGJS5tpPRHSnUPOnbEvwt70p2fYVGS6q12wL5OesO17MmF82qijwuinleL9K4B4BxMGDHc6eYjfSFN6aq1IXFMIGu3LylDa6QSlTNxZy+Jdb2yP4SuRtFWlToZ8tMIxHmBylfZ9mKeoILXCphNzU63HKbTsVSke8mE/vM1PnKVTFvqJiE9H856P8U030WW1uzUfEZs9BqjqApJOgmx0RQq0+JZGe4ubMjg9PMSvtq0zSKJgqUwyN7yESttVWs+JmJPXdYyxFvPctRl5zY9qRXYm/eQqSNRfmJRo06tPh0w70qRLu3hxMeQ6TbNkNJ8LFcWEHu56zNTEbEOzUILZdii5YZ76zWFpTTEZsezVR3gjE4WD0yCt1P3T1m0VaNPZ+BTx2x4jjGnK1ozFjAhKkjlAA1I9V/acNiiKciGt+eccEonkt/1lVTjVQNLLKgAayzMGbLtFioe7JiuyXyaN65C1Y2DDi8Kn0++TztNqo4HbMHDzGhlFrNbsVKuL5dmiLIN9U3czYNnxuoKsV9/DyvzlepXp0TTrbQjuuQwuca/OVnu0XDfvaREZXBByOnn5RQOQy/24gpH9v0nCIt/b+ksw8j/nUzBhuyrnyHTzmBipc/rzgNjebNVDhKbKzi/q1XLNvPpPpBNnwgIaVw5AVPu/F5wjC0U3G4wixI7IyG5jYRRdhFZlIKsQeoym07RUqd5zcga2/fcuBrKVz6iKuVhmPLQ/4iJbeQDHFr3089PxlSmzNfFl1bKNYHI3mzORobEabq47wMoHu76o7++goK6c5Yb6vgMo+PdX8MGRiuGPgUHyOGU1zuenO38dmr4byoEPvqPxLGOqjwtf8LSh4920aD5zZ/e32ENFDyjCzEQLWVL2ylq5F7ftL1rDXM2haqGw3N4RWs1xprFWoCLDXSf8i9rftGSubAiJTq+ILC9bmB+Qinac8IH5CY9r/wBtDtG0KbEwvtYFz/ExbVa/+ItTaW8JmLa72uYeOzYSLmYaoa2EXgasRcdbaCHi5gjTOKta2JVmOthxcovpDC4imu17S9fPy1yE4VVs8OsuWGJdcNiplP7Hv2y0lPNf7jBfii+uKC2Krf4RL8NqVx7tpVxoEs2XIwk+lgX5fxEZsFfPSfZUf+38xSfSXF5SN6NS72z1n2o718xnLk1aw+CP9VT9Zh7v5yiSKi584hvWr3br+ETI1LPi9XrKDOXS40XKIFC08LX9aI4vjYfcIM0q0R8Eq/U/+k09GEbwVOV6msu3GwvbvLyld/WEDllPSE1w961pxVvS17onH0y9+8ZwauIDnGrKRU1ztb8J6SManDKlRSqqoyENYcfHaLUAWqPvSnVQIFcaG4i1hxS5Ep1UVCrLe8YjFdRaNXSzEKcTC0qOGVB0ERsLgxayh6hIyaLVpq2S90jMQV0DrYGwESqFUDo+Kcf6zLxRa6WUlTiAtBWTBhdSc7xa1Owup7ukWstnDjIm8aspqq9shHOJ2PUzivOK84rzivOK84rzivOK84rzivOK84rzivOK84rzivOK84rzivOK84rzivOK84rzivOK84r9oGxhUFlPKCxL+QlPxiBb1W8jG0DgSy+OLzciYbVPnFAW+WpyhXCh8zlMIthty1lPIVMtIO+DcCU17uIiVFs3tUYhIhyf5Rc6eLmJiOEnrEYm984D6siMxCrbKYjglRjlC2YnEbHrzgOZ+Uc2GQ1jsRa0JvTz7f8A/8QAKRABAAIBAwQCAgIDAQEAAAAAAQARITFRYRBBcZGBsSAwodHh8PFAwf/aAAgBAQABPyH9CKguIij1ubiOCcSNdR6FW/8A4RY7wahKnciVh6DD5+v+9BXv99KwQEVA2ou//Bh4ENDpckgH9UAaHTVA/EX2rxB8dFi5/bfbumUMkBWiOrDx+tAU5I/QqArQXM1hxKA/ov0Vg9Mii/xsiWvun/UJR/EahBMdGM7/AKbbe344kyzTMTVh89RTSb18x2moBh/BULLH0/QCoHeHy9bApdVgi2Fyy7LS9g3YPmYYO9k1FdANb0sgjApomaoTfZLRDpDC5MCGWovQE4S7mvFnEWy+IXs9SQi09u35EiveV3eoNuXVwsvT8S40oUqht6BYE7RqwCjf3miX4whlmtB5DXLiFjXlUlFOFN5i6UUhpsYY8BX1g6Ed+EXy7lV3WT7Sqh8773xvCoyFmifIxPFEEU69R5pf/mYH9IikdTrW3z+C0RuEjwmxQIox87QwbHeAFBd8GXqbEYbw9fjY8RVbWIAo06O8SJMaJU0bCFyDRx/Uh17kLgo2+7AjWRVAzBg8kgi/goaxI9Qo2RJTTrjXfE8SS+v8mHyriF1u9yFBgH8y8OnYmhtqO8oIzX5UatL58IpHffMPswQeyzLZrHlnKElIADFUbyofTNe7SvXslAL6r7DcnGHqoFxnR+L+Ue+vhGIXe+hacTwGK6bXjzNThiMbOfMwct1awEaGXaQfOo7o6viYt6j/AD6/JAAsqh7rMS9Bzs5eA7RD0hewO6+8vBRvqXEchM0PVXcSPYu9T4s77/gfcZwdHDKt6aJxY1+JoNseuli7Z9SrPdzCyPRLfxAq9LiF+Gr0Yo7MvshJoTHd5Du5gHVXd79RaS49FO7stvu8SzdHafBA0rb1U1IRkNJ0r3Eursp1qfIdUqhy7Svt1VeP7xBZ4H+uj08kVDjD3jTAGGBms6Acnvs/EGwmjjHuU98ohVdnOCqm5/noM0P6dVNSXGjxOAlJWO/oMBAKTDUWpdKHmWRoMeYwPgC95q2nY4iA6yYxHwh6QafTgQwlo3IDBZpoIOp3OIYUU4JpmPEDi2EvSHW1raiK2WVxCDCLzUxvFvQgihHyEvSsbFE7MK2PqIC2TV0TY34I2L410PuIFwgd9Or3F3CeAawFZFfzhL0pGDm2Np2l+4q+lfYVF2DFlZrTM1wm69c8zPCrY+UJm6KzprFV3/VoS4qrr1GY3yXtKE/tksfT+n+ZTU7Jwh0IsXEsABViiLdnhxBSoS2+kHqevVSh2nmpaLp/T/EIVN3/ANltLRfuppB0Hyqay0MM4g0vEaG6/imarQMTVFj15x/MskCikJQyyKJ0CPztHzG9Ka6qqviogB2RZLpEM4pNCMcC3mjpiiJyhG1VUFQ6TCT0GM1bMeYlXKNkUMLm8qc2PpK0bVo4xUGgU0ackaqzB5YmUoXRJY4Bkgg9x9zkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0TkPROQ9E5D0flQOzDBq1+P7mhzR+Ybt5mmMJ9yuB12dpecCVdJrBwRpYKx07xa3+1UpAW0fLT66aDkCx34la6TuQhWLo/moz4q/vv+1bXa/wC5VyRnPQQgRsKVfmDlfKL/ALO5BxfCX3XI4fEvdtNkZ0sXX1EdSu3tOxmRf1KoAimqvHqOWpO/5//EACkQAQACAgECBgMBAQEBAQAAAAEAESExUUFhEHGBkaHRILHBMEDx4fD/2gAIAQEAAT8Q/wAAilwQEKRpPE1+gPVb/kAjnGdzvV+x8VCJUQKY/wCFOUF0lNJAY6J8ERIIjSPh59B7PB5ur1C3wJ6urDaMEe50F1n39P8Agdnj98+MeBrPmv8AWD+4XRHl4O29yhZrX3J/tytnPV8KuYX2H7/0BUAtYb7hgGqij/TIABVaA2sYdQRBAo/ywmqpxmXuqHkJ0BbALacNx4fk/wCAKlg55pw13hmVrpEEpgeZNv47MzTrzBB2iMCsccwR1+DuSiF5+ATUU4/a/wCN8mf1/BQl+nbJciD2y/MRvzMvigKR5Irt2ZfO5SF3kyQQkn4C4mDrFz1aDx/gbFqAO7CTtZXnv9eLwhFwnh6pKI/J6EK3GAGuYpog1zXttXQmw4NXTlcDIQHOFgrW+EIBgEspBrM3gVHJmD3S1aDrF+wXlaZUFp2PiqeonvHzHk/JCpSCuwfcejA2axosr8Jp4+LacTKKXT7lxv5UgMIN2mofCwkgRLN0RUVrijYUi9BLqWDrjOKHaUpGluKSRdFh3gV7E3Dlblb1lg/zhyMyeBrLoUmWlxtzuRjDSuE7psBtRAfF2a6WHUft+4lFf/75wyqRE4TxMyhux81v8ARY6Bze7z9Q85wWV3GPQ9wTV5LaxDsvYEVA7PKR5axtJwDHEQgQrtYN1bYYpumVXAV8g1FAKJepXCIxS68nZKOWM1tZ9sB7rTR/LhMtDgZCRlhL/VOiPskJr0/A20BNbdA+niAREbEhEGQDz458zf7fDN2a17vQjK4p6yGCgIjsToXff1ZqQPQlZzTxB6ymcukM/VmHrIsVmKXBetZTvAHdjYAR6W3MW6y3h3RB61/JEhhCrbNa6Yqw9W3syR7b+2LoDEW48Hn/APfF0nARI6ft+OhZS3nl+q8FAV0RuN+vfzLeSFTKAGcC47aVPO5QgcU+tJk3fXc155xKB50u8vS8MruoujqgbXcMrKSoEX1VxaXmBhoWOp+xYpCyFzHZXA0ucMwbzdmtnlOhKHrbEpD7mbuDpycQn3ZWlbqbJgKJw2cjgFLhoF3fIMMfSCHF5r014AXZfWuaj293+x/Ef/BMPAE9BXcymywPo2xn22uL2YB1/BwdQM3gUbqZT+zlSH9UNtwTdpY28gFM00veseLEQsaezcfA7FlYDC9xgYihQKUXNXW6npKwHVXWxwzqjNaxwmqgBqdoCe2f7P8AxQb/AL4txUHoU/J4pjqLQ6CGoTyK8bn5IXvp8g+3g4OXtZYoYkGhY1yR9c+684xsWAk6An5RM+Y/G01a4bbDktRI6m2BCt1qLnbWkPS55qT2T+PDyoD3GJY7/q+JqAjsdMKRZzhAI0Z6MTVkbqrmm2o3dYAn0gI0nuvFSnNcZG9Jc+CXwhwt2y1bNsWbXmOgXGlCG6YyyqtcbQV12JZIq0WxxpjfZdzWGqj5VOtoL/sE+aLOSGWpWc5c6Lm3d6m4PbEDhRj69S6VXCDCTXXUe1jAqjVrogEFYKfGvKFgACZHo1EpSCsZV3iMqBdFqdlFBWo6iNlzz3eVxKmurYHjSPojHoPiGL6xwXtG6aFnq0P09Yt6jtjWXcUHUxKEKezL3rNfGdoQC/b3BGi1Q4DQdMRWFkA1OhEQpS5bAsqzG2FbdMQxVurA5hXkausqiM9AtQpguWUr9xzBANwU6MD+pk1el2QMNJCFRAKD5wtIJVaXshQgCdii6yWVrgVomx13VSyks2UbIejheBk97l5Grwaci/hOgAE95L5QF11qH9ssH6KtQUTb2YElS63Bq856uYirjQ13fMQXPb57gA6q3F3yQIZ50lttA5cBN2ZfeGBLFRigZzFGUJ9D+kY/a81fV5iRqWlQ5cX9FWoYq9+8zybReRUG05gfywyJVVuuJYK+hTj6mTgqZukPuCWBV06BuHKN480GPaEoo1CHc09OHFYwdZ88sHSceGzXftHu3YSUxL30j5zZcsFmTSVWW5kqMSiy/cAVrs4dXmKvmDceNnEGhyYF63XvDEQHHhX/ANbHXXXXXXXXXXXXXXXXXXXXXXXXR+XTU40NOmofthdDYLNdRjzi1O4QM5K++oQgI/wzBDghMXoQqsOk3as+v7gHiGUoTFFpyQjiNOD7Gi/OE7EIHDrKO+JlUsqg2Np9aYGTRKyir4KUYl5tOl1fnefKZ7NQDSGPHg0FbH6jYgsBbTo3n6l6EJaNAwD/AFsJsDtRT2YxgMoxUE9AEp95RYIGG0RY4a2XYiQFYpv5T+RJYOSaD7WVFg6lqKsfmNpS1RLuMPla+HSJaqdVwHVSnOVrddxTthWj0h65B2TGKfEbaaHVhr8//9k=" alt="SRK Logo">
  </div>
  <div class="header-text">
    <h1>SRK INSTITUTE OF TECHNOLOGY</h1>
    <p>AN AUTONOMOUS INSTITUTION · ACCREDITED BY NBA &amp; NAAC WITH "A" GRADE · ENIKEPADU, VIJAYAWADA – 521 108</p>
  </div>
</header>

<main>

  <!-- ── STUDENT INFO ── -->
  <div class="card">
    <div class="card-title"><span>👤</span> Student Information</div>

    <div class="two-col">
      <div class="field-group">
        <label for="firstName">First Name</label>
        <input type="text" id="firstName" placeholder="e.g. Ravi">
      </div>
      <div class="field-group">
        <label for="lastName">Last Name</label>
        <input type="text" id="lastName" placeholder="e.g. Kumar">
      </div>
    </div>

    <div class="two-col">
      <div class="field-group">
        <label for="rollNo">Roll Number</label>
        <input type="text" id="rollNo" placeholder="e.g. 22B91A0501">
      </div>
      <div class="field-group">
        <label for="batch">Batch / Year</label>
        <input type="text" id="batch" placeholder="e.g. 2022–2026">
      </div>
    </div>

    <div class="field-group">
      <label for="department">Department</label>
      <select id="department">
        <option value="">-- Select Department --</option>
        <option>Computer Science &amp; Engineering (CSE)</option>
        <option>Electronics &amp; Communication Engineering (ECE)</option>
        <option>Electrical &amp; Electronics Engineering (EEE)</option>
        <option>Mechanical Engineering (ME)</option>
        <option>Civil Engineering (CE)</option>
        <option>Information Technology (IT)</option>
        <option>Artificial Intelligence &amp; Machine Learning (AI&amp;ML)</option>
        <option>Data Science (DS)</option>
        <option>Chemical Engineering</option>
        <option>Other</option>
      </select>
    </div>

    <div class="field-group">
      <label for="branch">Branch / Section</label>
      <input type="text" id="branch" placeholder="e.g. CSE-A or Section B">
    </div>
  </div>

  <!-- ── CGPA CALCULATOR ── -->
  <div class="card">
    <div class="card-title"><span>🎓</span> CGPA Calculator</div>
    <p style="font-size:13px;color:rgba(255,255,255,0.55);margin-bottom:18px;">Calculate your CGPA instantly for up to 8 semesters</p>

    <label for="numSem">Select Number of Semesters</label>
    <select id="numSem" onchange="renderSgpaInputs()">
      <option value="1">1 Semester</option>
      <option value="2" selected>2 Semesters</option>
      <option value="3">3 Semesters</option>
      <option value="4">4 Semesters</option>
      <option value="5">5 Semesters</option>
      <option value="6">6 Semesters</option>
      <option value="7">7 Semesters</option>
      <option value="8">8 Semesters</option>
    </select>

    <div id="sgpaInputs" class="sgpa-grid" style="margin-top:16px;"></div>

    <button class="btn btn-primary" onclick="calculateCGPA()">CALCULATE CGPA</button>

    <div class="result-box" id="cgpaResult">
      <div class="result-label">YOUR CGPA</div>
      <div class="result-value" id="cgpaValue">—</div>
      <div class="result-grade" id="cgpaGrade"></div>
    </div>
  </div>

  <!-- ── TARGET CGPA PLANNER ── -->
  <div class="card">
    <div class="card-title"><span>🎯</span> Target CGPA Planner</div>
    <label for="targetCGPA">Target CGPA</label>
    <input type="number" id="targetCGPA" placeholder="e.g. 9.0" step="0.01" min="0" max="10">
    <button class="btn btn-orange" onclick="calculateTarget()" style="margin-top:14px;">CALCULATE REQUIRED SGPA</button>
    <div class="target-result" id="targetResult">
      <p id="targetText"></p>
    </div>
  </div>

  <!-- ── DOWNLOAD PDF ── -->
  <div class="card">
    <div class="card-title"><span>📄</span> Download Report</div>
    <p style="font-size:13px;color:rgba(255,255,255,0.55);margin-bottom:18px;">Download your CGPA report as a PDF with all student details.</p>
    <button class="btn btn-green" onclick="downloadPDF()">⬇ DOWNLOAD PDF REPORT</button>
  </div>

  <!-- ── HISTORY ── -->
  <div class="card">
    <div class="card-title"><span>📋</span> Calculation History</div>
    <ul class="history-list" id="historyList">
      <li><span class="empty-history">No calculations yet.</span></li>
    </ul>
    <button class="btn-clear" onclick="clearHistory()">Clear History</button>
  </div>

</main>

<footer>© 2026 SRK Institute of Technology, Vijayawada · Built for students</footer>

<script>
  // ── Render SGPA inputs
  function renderSgpaInputs() {
    const n = parseInt(document.getElementById('numSem').value);
    const grid = document.getElementById('sgpaInputs');
    grid.innerHTML = '';
    for (let i = 1; i <= n; i++) {
      grid.innerHTML += `
        <div class="sgpa-item">
          <label for="s${i}">Semester ${i} SGPA</label>
          <input type="number" id="s${i}" placeholder="e.g. 8.5" step="0.01" min="0" max="10">
        </div>`;
    }
  }

  // ── Calculate CGPA
  let lastCGPA = null;
  function calculateCGPA() {
    const n = parseInt(document.getElementById('numSem').value);
    let total = 0, count = 0;
    for (let i = 1; i <= n; i++) {
      const v = parseFloat(document.getElementById('s' + i).value);
      if (!isNaN(v) && v >= 0 && v <= 10) { total += v; count++; }
    }
    if (count === 0) { alert('Please enter at least one valid SGPA.'); return; }
    const cgpa = (total / count).toFixed(2);
    lastCGPA = cgpa;
    document.getElementById('cgpaValue').textContent = cgpa;
    const grade = getGrade(parseFloat(cgpa));
    const gradeEl = document.getElementById('cgpaGrade');
    gradeEl.textContent = grade.label;
    gradeEl.style.background = grade.bg;
    gradeEl.style.color = grade.color;
    document.getElementById('cgpaResult').classList.add('show');
    addHistory(cgpa, count);
  }

  function getGrade(cgpa) {
    if (cgpa >= 9)   return { label: '🏆 Outstanding – O Grade',   bg: 'rgba(0,198,224,0.2)',  color: '#00e5ff' };
    if (cgpa >= 8)   return { label: '⭐ Excellent – A+ Grade',     bg: 'rgba(0,200,83,0.2)',   color: '#69f0ae' };
    if (cgpa >= 7)   return { label: '✅ Very Good – A Grade',       bg: 'rgba(100,221,23,0.2)', color: '#b2ff59' };
    if (cgpa >= 6)   return { label: '👍 Good – B+ Grade',           bg: 'rgba(255,214,0,0.2)',  color: '#ffd600' };
    if (cgpa >= 5)   return { label: '📘 Average – B Grade',         bg: 'rgba(255,145,0,0.2)',  color: '#ffab40' };
    return              { label: '⚠️ Below Average',                  bg: 'rgba(232,93,4,0.2)',   color: '#ff7043' };
  }

  // ── Target CGPA Planner
  function calculateTarget() {
    const n = parseInt(document.getElementById('numSem').value);
    const target = parseFloat(document.getElementById('targetCGPA').value);
    if (isNaN(target) || target < 0 || target > 10) { alert('Enter a valid target CGPA (0–10).'); return; }
    let total = 0, count = 0;
    for (let i = 1; i <= n; i++) {
      const v = parseFloat(document.getElementById('s' + i).value);
      if (!isNaN(v) && v >= 0 && v <= 10) { total += v; count++; }
    }
    const totalSems = 8;
    const remaining = totalSems - count;
    if (remaining <= 0) {
      document.getElementById('targetText').innerHTML = 'You have completed all 8 semesters!';
    } else {
      const requiredSgpa = ((target * totalSems - total) / remaining).toFixed(2);
      if (requiredSgpa > 10)
        document.getElementById('targetText').innerHTML = `Target CGPA of <span>${target}</span> is not achievable. Aim for a realistic goal. 💪`;
      else if (requiredSgpa < 0)
        document.getElementById('targetText').innerHTML = `Great news! You've already exceeded your target CGPA of <span>${target}</span>! 🎉`;
      else
        document.getElementById('targetText').innerHTML = `You need an average SGPA of <span>${requiredSgpa}</span> in the remaining <span>${remaining}</span> semester(s) to reach CGPA <span>${target}</span>.`;
    }
    document.getElementById('targetResult').classList.add('show');
  }

  // ── History
  let history = JSON.parse(localStorage.getItem('cgpa_history') || '[]');
  function addHistory(cgpa, sems) {
    const entry = { cgpa, sems, date: new Date().toLocaleDateString('en-IN') };
    history.unshift(entry);
    if (history.length > 10) history.pop();
    localStorage.setItem('cgpa_history', JSON.stringify(history));
    renderHistory();
  }
  function renderHistory() {
    const ul = document.getElementById('historyList');
    if (history.length === 0) { ul.innerHTML = '<li><span class="empty-history">No calculations yet.</span></li>'; return; }
    ul.innerHTML = history.map((h, i) => `
      <li>
        <div>
          <div style="font-weight:700;font-size:14px;">Calculation #${history.length - i}</div>
          <div class="h-sem">${h.sems} Semester(s) · ${h.date}</div>
        </div>
        <div class="h-cgpa">${h.cgpa}</div>
      </li>`).join('');
  }
  function clearHistory() {
    history = []; localStorage.removeItem('cgpa_history'); renderHistory();
  }

  // ── PDF Download using jsPDF
  function downloadPDF() {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });

    // Student info
    const firstName  = document.getElementById('firstName').value.trim() || '—';
    const lastName   = document.getElementById('lastName').value.trim()  || '—';
    const rollNo     = document.getElementById('rollNo').value.trim()    || '—';
    const batch      = document.getElementById('batch').value.trim()     || '—';
    const department = document.getElementById('department').value       || '—';
    const branch     = document.getElementById('branch').value.trim()    || '—';
    const cgpa       = lastCGPA || 'Not calculated';

    const W = 210, pageH = 297;
    // Navy background header band
    doc.setFillColor(26, 42, 94);
    doc.rect(0, 0, W, 42, 'F');

    // Orange accent stripe
    doc.setFillColor(232, 93, 4);
    doc.rect(0, 42, W, 3, 'F');

    // College name
    doc.setTextColor(255, 255, 255);
    doc.setFont('helvetica', 'bold');
    doc.setFontSize(16);
    doc.text('SRK INSTITUTE OF TECHNOLOGY', W / 2, 16, { align: 'center' });
    doc.setFontSize(8);
    doc.setFont('helvetica', 'normal');
    doc.text('AN AUTONOMOUS INSTITUTION | ACCREDITED BY NBA & NAAC WITH "A" GRADE', W / 2, 24, { align: 'center' });
    doc.text('ENIKEPADU, VIJAYAWADA - 521 108, A.P. INDIA', W / 2, 30, { align: 'center' });

    // Report title
    doc.setFillColor(0, 198, 224);
    doc.setTextColor(26, 42, 94);
    doc.setFont('helvetica', 'bold');
    doc.setFontSize(13);
    doc.text('CGPA REPORT', W / 2, 38, { align: 'center' });

    // Generated date
    doc.setTextColor(100, 100, 120);
    doc.setFont('helvetica', 'normal');
    doc.setFontSize(8);
    doc.text('Generated on: ' + new Date().toLocaleDateString('en-IN', { day:'2-digit', month:'long', year:'numeric' }), W - 14, 52, { align: 'right' });

    // ── Student Details Box
    let y = 58;
    doc.setFillColor(240, 244, 255);
    doc.roundedRect(14, y, W - 28, 62, 4, 4, 'F');
    doc.setDrawColor(26, 42, 94);
    doc.setLineWidth(0.5);
    doc.roundedRect(14, y, W - 28, 62, 4, 4, 'S');

    doc.setFont('helvetica', 'bold');
    doc.setFontSize(10);
    doc.setTextColor(26, 42, 94);
    doc.text('STUDENT DETAILS', 20, y + 9);

    doc.setDrawColor(232, 93, 4);
    doc.setLineWidth(0.8);
    doc.line(20, y + 11, 80, y + 11);

    // Two-column layout for student fields
    const fields = [
      ['Name', firstName + ' ' + lastName],
      ['Roll Number', rollNo],
      ['Department', department],
      ['Branch / Section', branch],
      ['Batch', batch],
    ];

    doc.setLineWidth(0.3);
    doc.setDrawColor(200, 210, 230);
    let fy = y + 18;
    fields.forEach(([lbl, val], idx) => {
      const col = idx % 2 === 0 ? 20 : 115;
      if (idx % 2 === 0 && idx > 0) fy += 12;
      doc.setFont('helvetica', 'bold');
      doc.setFontSize(7.5);
      doc.setTextColor(100, 120, 160);
      doc.text(lbl.toUpperCase(), col, fy);
      doc.setFont('helvetica', 'normal');
      doc.setFontSize(10);
      doc.setTextColor(26, 42, 94);
      doc.text(val, col, fy + 5.5);
    });

    // ── SGPA Table
    y = 130;
    const n = parseInt(document.getElementById('numSem').value);
    const sgpaRows = [];
    let total = 0, count = 0;
    for (let i = 1; i <= n; i++) {
      const v = parseFloat(document.getElementById('s' + i).value);
      if (!isNaN(v) && v >= 0 && v <= 10) { sgpaRows.push(['Semester ' + i, v.toFixed(2)]); total += v; count++; }
      else sgpaRows.push(['Semester ' + i, '—']);
    }

    doc.setFont('helvetica', 'bold');
    doc.setFontSize(10);
    doc.setTextColor(26, 42, 94);
    doc.text('SEMESTER-WISE SGPA', 14, y);
    doc.setDrawColor(232, 93, 4);
    doc.setLineWidth(0.8);
    doc.line(14, y + 2, 80, y + 2);

    y += 8;
    // Table header
    doc.setFillColor(26, 42, 94);
    doc.rect(14, y, 90, 8, 'F');
    doc.setFont('helvetica', 'bold');
    doc.setFontSize(9);
    doc.setTextColor(255, 255, 255);
    doc.text('Semester', 20, y + 5.5);
    doc.text('SGPA', 78, y + 5.5);
    y += 8;

    sgpaRows.forEach((row, i) => {
      doc.setFillColor(i % 2 === 0 ? 245 : 235, i % 2 === 0 ? 248 : 242, i % 2 === 0 ? 255 : 252);
      doc.rect(14, y, 90, 8, 'F');
      doc.setFont('helvetica', 'normal');
      doc.setFontSize(9);
      doc.setTextColor(40, 40, 70);
      doc.text(row[0], 20, y + 5.5);
      doc.text(row[1], 78, y + 5.5);
      y += 8;
    });

    // ── CGPA Result box
    y += 10;
    doc.setFillColor(26, 42, 94);
    doc.roundedRect(14, y, W - 28, 32, 5, 5, 'F');
    doc.setFont('helvetica', 'bold');
    doc.setFontSize(11);
    doc.setTextColor(0, 198, 224);
    doc.text('FINAL CGPA', W / 2, y + 10, { align: 'center' });
    doc.setFontSize(26);
    doc.setTextColor(255, 255, 255);
    doc.text(String(cgpa), W / 2, y + 25, { align: 'center' });

    // Grade tag
    if (lastCGPA) {
      const gradeText = getGradeText(parseFloat(lastCGPA));
      y += 40;
      doc.setFillColor(0, 198, 224);
      doc.roundedRect(14, y, W - 28, 12, 3, 3, 'F');
      doc.setFont('helvetica', 'bold');
      doc.setFontSize(9);
      doc.setTextColor(26, 42, 94);
      doc.text(gradeText, W / 2, y + 8, { align: 'center' });
      y += 20;
    } else { y += 8; }

    // Footer
    doc.setFillColor(26, 42, 94);
    doc.rect(0, pageH - 16, W, 16, 'F');
    doc.setFont('helvetica', 'normal');
    doc.setFontSize(7.5);
    doc.setTextColor(180, 200, 240);
    doc.text('© SRK Institute of Technology, Vijayawada | Ph: +91 83749 56444', W / 2, pageH - 6, { align: 'center' });

    const name = (firstName + '_' + lastName).replace(/\s+/g, '_') || 'Student';
    doc.save('CGPA_Report_' + name + '.pdf');
  }

  function getGradeText(cgpa) {
    if (cgpa >= 9) return 'Outstanding — O Grade';
    if (cgpa >= 8) return 'Excellent — A+ Grade';
    if (cgpa >= 7) return 'Very Good — A Grade';
    if (cgpa >= 6) return 'Good — B+ Grade';
    if (cgpa >= 5) return 'Average — B Grade';
    return 'Below Average';
  }

  // Init
  renderSgpaInputs();
  renderHistory();
</script>
</body>
</html>
