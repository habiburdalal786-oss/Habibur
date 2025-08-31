<!DOCTYPE html>
<html>
<head>
    <title>Habibur Car Game</title>
    <style>
        body { margin:0; overflow:hidden; background:#cceeff; font-family:sans-serif;}
        #gameCanvas { display:block; margin:0 auto; background:#7ec850; }
        /* অ্যাডের জন্য প্লেসহোল্ডার */
        #adBanner { width:100%; height:60px; background:#ccc; text-align:center; line-height:60px; font-weight:bold; color:#333; }
    </style>
</head>
<body>

<!-- অ্যাড ব্যানার -->
<div id="adBanner">Your Ad Here</div>

<canvas id="gameCanvas" width="400" height="600"></canvas>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let score = 0;

// প্লেয়ার কার্টুন মানুষ
let player = { x:canvas.width/2-25, y:canvas.height-100, width:50, height:80, color:'orange', speed:5 };

// যানবাহন অ্যারে
let vehicles = [];
let vehicleTypes = [
    {width:50, height:100, color:'red'},    // BMW
    {width:40, height:80, color:'blue'},    // মোটরসাইকেল
    {width:30, height:60, color:'green'}    // সাইকেল
];

// কী বোর্ড
let left=false, right=false;
document.addEventListener('keydown', e=>{ if(e.key==='ArrowLeft') left=true; if(e.key==='ArrowRight') right=true; });
document.addEventListener('keyup', e=>{ if(e.key==='ArrowLeft') left=false; if(e.key==='ArrowRight') right=false; });

// নতুন যানবাহন
function addVehicle(){
    let type = vehicleTypes[Math.floor(Math.random()*vehicleTypes.length)];
    let x = Math.random()*(canvas.width-type.width-20)+10;
    vehicles.push({x:x, y:-type.height, width:type.width, height:type.height, color:type.color, speed:3+Math.random()*3});
}

// ক্র্যাশ চেক
function checkCrash(v){
    return player.x < v.x + v.width && player.x + player.width > v.x &&
           player.y < v.y + v.height && player.y + player.height > v.y;
}

// ড্র লুপ
function draw(){
    ctx.clearRect(0,0,canvas.width,canvas.height);

    // রাস্তা
    ctx.fillStyle = "#555";
    ctx.fillRect(50,0,canvas.width-100,canvas.height);

    // প্লেয়ার
    ctx.fillStyle = player.color;
    ctx.fillRect(player.x, player.y, player.width, player.height);

    // যানবাহন
    for(let i=0;i<vehicles.length;i++){
        let v = vehicles[i];
        ctx.fillStyle = v.color;
        ctx.fillRect(v.x,v.y,v.width,v.height);
        v.y += v.speed;

        if(v.y>canvas.height){
            vehicles.splice(i,1);
            score++;
            i--;
        }

        if(checkCrash(v)){
            alert("Game Over! Score: "+score);
            document.location.reload();
        }
    }

    // প্লেয়ার মুভ
    if(left && player.x>50) player.x -= player.speed;
    if(right && player.x<canvas.width-player.width-50) player.x += player.speed;

    // স্কোর
    ctx.fillStyle='black';
    ctx.font='20px Arial';
    ctx.fillText("Score: "+score,10,30);

    // মাঝে মাঝে যানবাহন
    if(Math.random()<0.02) addVehicle();

    requestAnimationFrame(draw);
}

draw();
</script>

</body>
</html>
