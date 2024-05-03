<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
</head>
<style>body{
	margin: 0;
	padding: 0;
	background-color: #333;
}
canvas{
	display: block;
	margin: 100px auto;
}
</style>


<body>

    <img id="model" src="anas.jpeg">
</body>



<script>window.onload = function(){

	var size = 600;
	var bloc = 5;

// image
	var img = document.getElementById("model");
	var imgW = img.width;
	var imgH = img.height;
	var coef = imgW / imgH;
	img.height = size;
	img.width = size * coef;


// canvas temporaire
	var tcanvas = document.createElement("canvas");
	var tctx = tcanvas.getContext("2d");
	tcanvas.height = tcanvas.width = size;

// canvas
	var canvas = document.createElement("canvas");
	canvas.setAttribute("id","dots");
	var ctx = canvas.getContext("2d");
	canvas.height = canvas.width = size;

// Creation des données
	tctx.drawImage(img,(-(img.width - size) / 2),0,img.width,img.height);
	var imgData = tctx.getImageData(0,0,tcanvas.width,tcanvas.height);
	var data = imgData.data;

	var particles = [];
	var nbr_particles = (size / bloc)*(size / bloc);
	var pos_x = 0;
	var pos_y = 0;
	var pix;
	var moy;

// Création des particules.
	for (var i = 0; i < nbr_particles; i++) {
		particles.push(new particle(i));
	};
	function particle(i){
		this.x = pos_x;
		this.y = pos_y;

		if((pos_x + bloc) < tcanvas.width){  
		  pos_x += bloc;
		}else{
		  pos_y += bloc;
		  pos_x = 0;  
		}

		pix = ((pos_y + Math.round(bloc/2)) * size) - (size - Math.round(pos_x + (bloc/2)));
		moy = Math.round((data[pix*4] + data[pix*4+1] + data[pix*4+2])/3)
		this.size = bloc * (moy/255);
	}

// Dessin !
	function draw(){
		for (var i = 0; i < particles.length; i++) {
			p = particles[i]
			ctx.fillStyle = "white";
			ctx.fillRect(p.x,p.y,p.size,p.size);
		};
	}


	draw();
	document.body.insertBefore(canvas,img);
 	img.style.display = "none";
}
</script>
</html>
