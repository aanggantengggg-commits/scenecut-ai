# scenecut-ai
AI YouTube Video Clipper

<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>SceneCut AI</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#090b11;color:white;font-family:Arial,sans-serif;padding:22px}
main{max-width:650px;margin:auto}
h1{font-size:36px}
h1 span{color:#a99aff}
p{color:#aab3c4;line-height:1.6}
.card{background:#151b27;padding:20px;border-radius:18px;border:1px solid #30394a}
input{width:100%;padding:16px;border-radius:10px;border:1px solid #39445a;background:#090e16;color:white;font-size:15px;margin:12px 0}
button{width:100%;padding:16px;border:0;border-radius:10px;background:linear-gradient(90deg,#a99aff,#65e4c7);font-weight:bold;font-size:16px}
.tag{display:inline-block;background:#242139;padding:8px;border-radius:8px;margin:10px 4px 10px 0;font-size:12px}
#status{margin-top:15px}
video{width:100%;margin-top:12px;border-radius:10px}
.clip{background:#202838;padding:14px;border-radius:12px;margin-top:15px}
a{color:#65e4c7}
</style>
</head>
<body>
<main>
<h1>SceneCut <span>AI</span></h1>
<p>AI YouTube Highlight Clipper</p>
<h1>Ambil momen penting.<br><span>Simpan sebagai clip.</span></h1>
<div class="card">
<label>Link video YouTube</label>
<input id="url" placeholder="Tempel link YouTube di sini">
<div>
<span class="tag">30–60 detik</span>
<span class="tag">16:9 Original</span>
<span class="tag">Multiple Clips</span>
</div>
<button id="go" onclick="makeClips()">BUAT CLIP</button>
<div id="status"></div>
<div id="results"></div>
</div>
</main>
<script>
async function makeClips(){
 const url=document.getElementById('url').value.trim();
 const status=document.getElementById('status');
 const results=document.getElementById('results');
 const btn=document.getElementById('go');
 if(!url){status.textContent='Tempel link YouTube dahulu.';return}
 btn.disabled=true;
 status.textContent='AI sedang menganalisis video. Mohon tunggu...';
 results.innerHTML='';
 try{
  const response=await fetch('/api/clip',{
   method:'POST',
   headers:{'Content-Type':'application/json'},
   body:JSON.stringify({url})
  });
  const data=await response.json();
  if(!response.ok)throw Error(data.detail||'Proses gagal');
  data.clips.forEach((clip,i)=>{
   const box=document.createElement('div');
   box.className='clip';
   const title=document.createElement('h3');
   title.textContent='Highlight '+(i+1);
   const video=document.createElement('video');
   video.controls=true;
   video.src=clip.url;
   const link=document.createElement('a');
   link.href=clip.url;
   link.download='SceneCut-Clip-'+(i+1)+'.mp4';
   link.textContent='Download MP4';
   box.append(title,video,document.createElement('br'),link);
   results.append(box);
  });
  status.textContent='Selesai! '+data.clips.length+' clip berhasil dibuat.';
 }catch(error){
  status.textContent='Gagal: '+error.message;
 }finally{
  btn.disabled=false;
 }
}
</script>
</body>
</html>
