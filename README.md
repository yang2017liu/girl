# girl
test

<!DOCTYPE html>
<html>
	<head>
		<title> Girl </title>
		<link rel="stylesheet" type="text/css" href="./styles.css"/>
		<meta name="author" content="Yang Liu">
		<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
	</head>
	<body>
		<div class="image-container">
			<canvas class="canvas" width="900" height="1200" id="canvas"></canvas>		
            <div class ="loading-screen" id="loading"> Yang is on her way.... ;) </div>
			</div>
		</body>
		<script type="text/javascript" src="./main.js"></script>
</html>


------------------------------------------------------------------------------------------------
//Get reference to Canvas
var canvas = document.getElementById ('canvas');

// Get reference to Canvas Context
var context = canvas.getContext('2d');

//get reference to loading screen 
var loading_screen = document.getElementById('loading');

//Initialize Loading variables
var loaded = false;
var load_counter = 0;

//Initialize images for layers
var background = new Image();
var green_1 = new Image();
var green_2 = new Image();
var green_3 = new Image();
var people = new Image();
var green_4 = new Image();
var green_5 = new Image();
var green_6 = new Image();
var frame = new Image();
var green_7 = new Image();
var bird = new Image();

//Create a list of layer objects
var layer_list =[
  {
     'image':background,
     'src':'./images/layer_1_1.png',
     'z_index':0,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  {
     'image':green_1,
     'src':'./images/layer_2_1.png',
     'z_index':-1.25,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  {
     'image':green_2,
     'src':'./images/layer_3_1.png',
     'z_index':-1,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  
  {
     'image':green_3,
     'src':'./images/layer_4_1.png',
     'z_index':-0.3,
     'position':{x:0, y:0},
     'blend': 'multiply',
     'opacity':1
  },
  {
     'image':people,
     'src':'./images/layer_5_1.png',
     'z_index':-0.6,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  {
     'image':green_4,
     'src':'./images/layer_6_1.png',
     'z_index':0.5,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  {
     'image':green_5,
     'src':'./images/layer_7_1.png',
     'z_index':1,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  {
     'image':green_6,
     'src':'./images/layer_8_1.png',
     'z_index':1.5,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
  {
     'image':frame,
     'src':'./images/layer_9_1.png',
     'z_index':0,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  },
   {
     'image':green_7,
     'src':'./images/layer_10_1.png',
     'z_index':2,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':0.9
  },
  {
  	'image':bird,
     'src':'./images/layer_11_1.png',
     'z_index':2.2,
     'position':{x:0, y:0},
     'blend': null,
     'opacity':1
  	
  	}
  
];




layer_list.forEach(function(layer,index){
	layer.image.onload = function(){
		load_counter += 1;
		if(load_counter >= layer_list.length){
			//hie the loading screen
			hideLoading();
			
			requestAnimationFrame(drawCanvas);
		}
		
	}
	layer.image.src= layer.src;
	
});

function hideLoading(){
	loading_screen.classList.add('hidden');
}
function drawCanvas(){
	// clear whatever is in the canvas
	context.clearRect(0,0,canvas.width,canvas.height);
	
	// calculate how much the canvas should rotate
	var rotate_x = (pointer.y * -0.15) + (motion.y * -1.2);
	var rotate_y = (pointer.x * 0.15) + (motion.x * 1.2);
	
	var transform_string ="rotateX(" + rotate_x * "deg) rotateY(" + rotate_y + "deg)";
	
	/// actually rotate the canvas
	canvas.style.transform = transform_string;
	
	
	//loop through each layer and draw it to the canvas
	layer_list.forEach(function(layer, index){
		
		layer.position = getOffset(layer);
		
		if(layer.blend){
			context.globalCompositeOperation = layer.blend;
			}else{
				context.globalCompositeOperation ='normal';
			}
			context.globalAlpha = layer.opacity;
			
		context.drawImage(layer.image, layer.position.x, layer.position.y);
	});
	
	requestAnimationFrame(drawCanvas);
}

function getOffset(layer){
	var touch_multiplier = 3;
	var touch_offset_x = pointer.x * layer.z_index * touch_multiplier;
	var touch_offset_y = pointer.y * layer.z_index * touch_multiplier;
	
	var motion_multiplier = 2;
	var motion_offset_x = motion.x * layer.z_index * motion_multiplier;
	var motion_offset_y = motion.y * layer.z_index * motion_multiplier;
	
	
	var offset = {
		x:touch_offset_x + motion_offset_x,
		y:touch_offset_y + motion_offset_y
	};
	return offset;
}
/// touch and mouse controls ////

var moving = false;


// Initialize touch and mouse position
var pointer_initial = {
	x:0,
	y:0
	
};

var pointer = {
	x:0,
	y:0
};

canvas.addEventListener('touchstart', pointerStart);
canvas.addEventListener('mousedown', pointerStart);

function pointerStart (event){
	moving = true;
	if (event.type === 'touchstart'){
       //alert('touch');
		 pointer_initial.x = event.touches[0].clientX;
		pointer_initial.y = event.touches[0].clientY;
	} else if (event.type === 'mousedown'){
     //alert('mousedwon');
     pointer_initial.x = event.clientX;
	 pointer_initial.y = event.clientY;
		
	}
}

window.addEventListener('touchmove', pointerMove);
window.addEventListener('mousemove', pointerMove);

function pointerMove(event){
	event.preventDefault();
	if(moving === true ){
		var current_x = 0;
        var current_y = 0;
		if(event.type ==='touchmove'){
			current_x = event.touches[0].clientX;
			current_y = event.touches[0].clientY;
			
		}else if (event.type === 'mousemove'){
			current_x = event.clientX;
			current_y = event.clientY;
			
		}
		pointer.x = current_x - pointer_initial.x;
		pointer.y = current_y - pointer_initial.y;
	}
}



canvas.addEventListener('touchmove',function (event){
	event.preventDefault();
	
});

canvas.addEventListener('mousemove',function(event){
	event.preventDefault();
});

window.addEventListener('touchend',function(event){
	endGesture();
});

window.addEventListener('mouseup',function(event){
	endGesture();
});

function endGesture(){
 moving = false;
 
 pointer.x = 0;
 pointer.y = 0;
 }
 
 
 
 ///motion controls /////
 
 
 // Initialize variable for motion-base parallax
 
 var motion_initial ={
 	x:0,
 	y:0
 };
 
 var motion = {
 	x:0,
 	y:0
 };
 
 // listener to gyroscope events
 
 window.addEventListener('deviceorientation' , function (event){
 	// if this is the first time through
 	if(!motion_initial.x && !motion_initial.y){
 		 motion_initial.x = event.beta;
 		 motion_initial.y = event.gamma;
 	}
 	
 	if(window.orientation ===0){
 		
 	// the device is in portrait orientation
 	motion.x = event.gamma - motion_initial.y;
 	motion.y = event.beta - motion_initial.x;
 	}else if (window.orientation === 110){
 	// the device is in landscape on its left side 
 	motion.x = event.beta - motion_initial.x;
 	motion.y = event.gamma + motion_initial.y;
 	}else if(window.orientation === -70){
 	// the device is in landscape on its right side
 	motion.x = -event.beta + motion_initial.x;
 	motion.y = event.gamma - motion_initial.y;
 	}else{
 	// the device is upside down
 	motion.x = -event.gamma + motion_initial.y;
 	motion.y = -event.beta + motion_initial.x;
 	}
 });
 
 window.addEventListener('orientationchange',function(event){
 	
 	motion_initial.x = 0;
 	motion_initial.y = 0;
 });
 
 
 
 --------------------------------------------------------------------------------------------------------
 
 
 html,
body{
	box-sizing:border-box;
	margin:0;
	height:100;
	background:white;
	display:flex;
	justify-content:center;
	align-items:center;
	font-family:sans-serif;
}

.image-container{
	display:flex;
	height:100%;
	width:100%;
	max-width:100%;
	position:relative;
	
}

.canvas{
	width:auto;
	max-height:100%;
	max-width:100%;
	display:block;
	margin:auto;
	
}
.loading-screen{
	position: absolute;
	width: 100%;
	height:100%;
	background: white;
	top:0;
	bottom:0;
	left:0;
	right:0;
	visibility:visible;
	opacity:1;
	text-align:center;
	display:flex;
	flex-direction:column;
	aligh-items:center;
	justify-content:center;
	line-height:40px;
	transition:0.25s ease all 0.75s;
	color:#999;
	tranform:translateZ(50px);
}
.loading-screen.hidden{
	visibility:hidden;
	opacity:0;
}


