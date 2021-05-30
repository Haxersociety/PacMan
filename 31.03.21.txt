const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const bground = new Image();
bground.src = "res/background3.png";

const food2 = new Image();
food2.src = "res/food2.png";
const food1 = new Image();
food1.src = "res/food1.png";

const w0 = new Image();
w0.src = "res/w0.png";
const w1 = new Image();
w1.src = "res/w1.png";
const w2 = new Image();
w2.src = "res/w2.png";
const w3 = new Image();
w3.src = "res/w3.png";
const w4 = new Image();
w4.src = "res/w4.png";
const w5 = new Image();
w5.src = "res/w5.png";

const pacMan = new Image();
pacMan.src = "res/pacman1.png";

const ghostRed = new Image();
ghostRed.src = "res/ghostRed.png";

let cell = 20;
let food = {
	x: Math.floor(Math.random()*20)*cell+20,
	y: Math.floor(Math.random()*20)*cell+20,
};
let pacman = {
	x: 20,
	y: 10*cell,
};
let ghostR = {
	x: 10*cell,
	y: 10*cell,
};

// 0 - wall
// 1 - empty
// 3 - ghost spawn
matrixMap = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0],
	[0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0],
	[0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0],
	[0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0],
	[2, 2, 2, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 2, 2, 2],
	[0, 0, 0, 0, 1, 0, 1, 0, 0, 3, 0, 0, 1, 0, 1, 0, 0, 0, 0],
	[2, 2, 2, 2, 1, 1, 1, 0, 3, 3, 3, 0, 1, 1, 1, 2, 2, 2, 2],
	[0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0],
	[2, 2, 2, 0, 1, 0, 1, 1, 1, 2, 1, 1, 1, 0, 1, 0, 2, 2, 2],
	[0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0],
	[0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0],
	[0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0],
	[0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0],
	[0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
];

let ghRX = ghostR.x;
let ghRY = ghostR.y;
let score = 0;
let pacX = pacman.x;
let pacY = pacman.y;
document.addEventListener("keydown", direction);

let dir;
let dirL;
let wall = false;
function direction(){
	switch(event.keyCode){
		case 37:
			if(dir == "right")
				break;
			if(dir == "up" || dir == "down" && matrixMap[pacY/cell][pacX/cell-1] == 0){
				dirL = "left";
				break;
			}
			dir = "left";
			break;
		case 38:
			if(dir == "down")
				break;
			if(dir == "left" || dir == "right" && matrixMap[pacY/cell-1][pacX/cell] == 0){
				dirL = "up";
				break;
			}
			dir = "up";
			break;
		case 39:
			if(dir == "left")
				break;
			if(dir == "up" || dir == "down" && matrixMap[pacY/cell][pacX/cell+1] == 0){
				dirL = "right";
				break;
			}
			dir = "right";
			break;
		case 40:
			if(dir == "up")
				break;
			if(dir == "left" || dir == "right" && matrixMap[pacY/cell+1][pacX/cell] == 0){
				dirL = "down";
				break;
			}
			dir = "down";
			break;
		default:
			dir = "stop";
	}

};

let foods = []; 
for(let i = 0; i < 18; i++){
	for(let j = 0; j < 22; j++){
		foods[i*22 + j] = {
			x: i,
			y: j,
			dr: 1,
		}
	}
};

function drawGame(){
	ctx.drawImage(bground, 0, 0);
	ctx.drawImage(food2, food.x, food.y);
	for(let i = 0; i < 18; i++){
		for(let j = 0; j < 22; j++)
			if(foods[i*22 +j].dr == 1 && matrixMap[j][i] == 1)
				ctx.drawImage(food1, foods[i*22+j].x*cell, foods[i*22+j].y*cell);
				
	}
	///////////////// Прорисовка стен
	for(let i = 1; i < 18; i++){
		ctx.drawImage(w0, i*20, 0);
		ctx.drawImage(w0, i*20, 420);
	}
	for(let i = 1; i < 9; i++){
		ctx.drawImage(w1, 0, i*20);
		ctx.drawImage(w1, 360, i*20);
	}
	for(let i = 12; i < 21; i++){
		ctx.drawImage(w1, 0, i*20);
		ctx.drawImage(w1, 360, i*20);
	}

	ctx.drawImage(w2,0,0);
	ctx.drawImage(w2,0,220);
	ctx.drawImage(w2,0,140);
	ctx.drawImage(w2,0,260);
	ctx.drawImage(w2,0,340);
	ctx.drawImage(w3,360,0);
	ctx.drawImage(w3,360,220);
	ctx.drawImage(w3,360,140);
	ctx.drawImage(w3,360,260);
	ctx.drawImage(w3,360,340);
	ctx.drawImage(w4,360,180);
	ctx.drawImage(w4,360,140);
	ctx.drawImage(w4,360,260);
	ctx.drawImage(w4,360,340);
	ctx.drawImage(w4,360,420);
	ctx.drawImage(w5,0,180);
	ctx.drawImage(w5,0,140);
	ctx.drawImage(w5,0,260);
	ctx.drawImage(w5,0,340);
	ctx.drawImage(w5,0,420);
	ctx.drawImage(w2,180,0);
	ctx.drawImage(w3,180,0);

	for(let i = 1; i < 18; i++){
		for(let j = 1; j < 21; ++j){
			if(matrixMap[j][i] == 0 && matrixMap[j+1][i] == 0 && matrixMap[j][i+1] == 0){
				ctx.drawImage(w2, i*20, j*20);
			}
			if(matrixMap[j][i] == 0 && matrixMap[j+1][i] == 0 && matrixMap[j][i-1] == 0){
				ctx.drawImage(w3, i*20, j*20);
			}
			if(matrixMap[j][i] == 0 && matrixMap[j-1][i] == 0 && matrixMap[j][i-1] == 0){
				ctx.drawImage(w4, i*20, j*20);
			}
			if(matrixMap[j][i] == 0 && matrixMap[j-1][i] == 0 && matrixMap[j][i+1] == 0){
				ctx.drawImage(w5, i*20, j*20);
			}
			if(matrixMap[j][i] == 0 && (matrixMap[j+1][i] == 0 || matrixMap[j-1][i] == 0) && matrixMap[j][i+1] != 0 && matrixMap[j][i-1] != 0){
				ctx.drawImage(w1, i*20, j*20);
			}
			if(matrixMap[j][i] == 0 && (matrixMap[j][i+1] == 0 || matrixMap[j][i-1] == 0) && matrixMap[j+1][i] != 0 && matrixMap[j-1][i] != 0){
				ctx.drawImage(w0, i*20, j*20);
			}

		}
	}
	/////////////////

	//ctx.drawImage(pacMan, pacman.x, pacman.y);
	if(dir == "left"){
		if(matrixMap[pacY/cell][pacX/cell-1] == 0){
			dir = "stop";
		}
		else
			pacX -= cell;
	}
	if(dir == "right"){
		if(matrixMap[pacY/cell][pacX/cell+1] == 0){
			dir = "stop";
		}
		else
			pacX += cell;
	}
	if(dir == "up"){
		if(matrixMap[pacY/cell-1][pacX/cell] == 0){
			dir = "stop";
		}
		else
			pacY -= cell;
	}
	if(dir == "down"){
		if(matrixMap[pacY/cell+1][pacX/cell] == 0){
			dir = "stop";
		}
		else
			pacY += cell;
	}
	if(matrixMap[pacY/cell][pacX/cell-1] != 0 && dirL == "left"){
		dirL = "0";
		dir = "left";
	}
	if(matrixMap[pacY/cell][pacX/cell+1] != 0 && dirL == "right"){
		dirL = "0";
		dir = "right";
	}
	if(matrixMap[pacY/cell-1][pacX/cell] != 0 && dirL == "up"){
		dirL = "0";
		dir = "up";
	}
	if(matrixMap[pacY/cell+1][pacX/cell] != 0 && dirL == "down"){
		dirL = "0";
		dir = "down";
	}
	if(dir == "left" && pacX == 0)
		pacX = 380;
	if(dir == "right" && pacX == 360)
		pacX = 0;
	ctx.drawImage(ghostRed, ghRX, ghRY);
	ctx.drawImage(pacMan, pacX, pacY);
	for(let i = 0; i < 18; i++){
		for(let j = 0; j < 22; j++)
			if(pacX == foods[i*22+j].x*cell && pacY == foods[i*22+j].y*cell && foods[i*22+j].dr == 1 && matrixMap[j][i] == 1){
				foods[i*22+j].dr = 0;
				score++;
				console.log(score);
			}
		
	}
}

let game = setInterval(drawGame, 100);