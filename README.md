<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>GALAXIA ULTRA REALISTA</title>
<style>
    body{ margin:0; overflow:hidden; background:black; cursor:none; }
    canvas{ display:block; }
</style>
</head>
<body>
<canvas id="universe"></canvas>
<script>
const canvas = document.getElementById("universe");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
let cx = canvas.width/2;
let cy = canvas.height/2;
let mouseX = cx;
let mouseY = cy;

class Star{
    constructor(){ this.reset(); }
    reset(){
        this.x = (Math.random()-0.5)*2000;
        this.y = (Math.random()-0.5)*2000;
        this.z = Math.random()*2000;
    }
    update(){
        this.z -= 4;
        if(this.z<=1) this.reset();
    }
    draw(){
        let sx = (this.x/this.z)*800 + cx;
        let sy = (this.y/this.z)*800 + cy;
        let size = (1-this.z/2000)*3;
        ctx.beginPath();
        ctx.arc(sx,sy,size,0,Math.PI*2);
        ctx.fillStyle="white";
        ctx.fill();
    }
}

let stars=[];
for(let i=0;i<2000;i++) stars.push(new Star());

class GalaxyStar{
    constructor(){
        this.angle=Math.random()*Math.PI*2;
        this.radius=Math.random()*500;
        this.speed=0.0005+Math.random()*0.001;
        this.size=Math.random()*2;
    }
    update(){ this.angle+=this.speed; }
    draw(){
        let x=cx+Math.cos(this.angle)*this.radius;
        let y=cy+Math.sin(this.angle)*this.radius;
        let gradient=ctx.createRadialGradient(x,y,0,x,y,4);
        gradient.addColorStop(0,"white");
        gradient.addColorStop(1,"transparent");
        ctx.fillStyle=gradient;
        ctx.beginPath();
        ctx.arc(x,y,this.size+1,0,Math.PI*2);
        ctx.fill();
    }
}

let galaxy=[];
for(let i=0;i<1500;i++) galaxy.push(new GalaxyStar());

class Planet{
    constructor(radius,color,size,speed){
        this.angle=Math.random()*Math.PI*2;
        this.radius=radius;
        this.color=color;
        this.size=size;
        this.speed=speed;
    }
    update(){ this.angle+=this.speed; }
    draw(){
        let x=cx+Math.cos(this.angle)*this.radius;
        let y=cy+Math.sin(this.angle)*this.radius;
        let g=ctx.createRadialGradient(x,y,1,x,y,this.size);
        g.addColorStop(0,this.color);
        g.addColorStop(1,"black");
        ctx.fillStyle=g;
        ctx.beginPath();
        ctx.arc(x,y,this.size,0,Math.PI*2);
        ctx.fill();
    }
}

let planets=[
    new Planet(150,"cyan",8,0.01),
    new Planet(250,"purple",12,0.006),
    new Planet(350,"orange",16,0.004)
];

class ShootingStar{
    constructor(){ this.reset(); }
    reset(){
        this.x=Math.random()*canvas.width;
        this.y=0;
        this.len=200;
        this.speed=10+Math.random()*10;
        this.active=Math.random()<0.02;
    }
    update(){
        if(!this.active){
            if(Math.random()<0.002) this.active=true;
            return;
        }
        this.x+=this.speed;
        this.y+=this.speed;
        if(this.y>canvas.height){ this.reset(); }
    }
    draw(){
        if(!this.active) return;
        ctx.strokeStyle="white";
        ctx.lineWidth=2;
        ctx.beginPath();
        ctx.moveTo(this.x,this.y);
        ctx.lineTo(this.x-this.len,this.y-this.len);
        ctx.stroke();
    }
}

let meteors=[];
for(let i=0;i<5;i++) meteors.push(new ShootingStar());

document.addEventListener("mousemove",e=>{
    mouseX=e.clientX;
    mouseY=e.clientY;
});

function animate(){
    cx += (mouseX-cx)*0.02;
    cy += (mouseY-cy)*0.02;
    ctx.fillStyle="rgba(0,0,10,0.4)";
    ctx.fillRect(0,0,canvas.width,canvas.height);
    stars.forEach(s=>{s.update();s.draw();});
    galaxy.forEach(g=>{g.update();g.draw();});
    planets.forEach(p=>{p.update();p.draw();});
    meteors.forEach(m=>{m.update();m.draw();});
    requestAnimationFrame(animate);
}
animate();

window.addEventListener("resize",()=>{
    canvas.width=window.innerWidth;
    canvas.height=window.innerHeight;
});
</script>
</body>
</html>
