## someone328: bullets with trigger

```javascript
	map.defineObject('MyBullet', { //only way to kill the boss is using "projectile" object
        'type': 'dynamic',
        'symbol': '.',
        'color': 'green',
        'interval': 100,
		'projectile': true,
        'behavior': function (me) {
            me.move('right');
        }
    });
	//create trigger for open fire
	map.defineObject('trigger', {
        'symbol': '=',
        'color': 'green',
		'onCollision': function (player){
        	for(i=0; i<26; i+=2){
				map.placeObject(i, 5, 'MyBullet');
				map.placeObject(i, 6, 'MyBullet');
            }
		},
    });
	//place trigger in safe place near the player
	map.placeObject(1, map.getHeight() - 3, 'trigger');
```

## Kebabbi: heat-seeking missiles

```javascript
var bosses = map.getDynamicObjects();
 
for(var i=0;i<bosses.length;i++){
        (function(k){
                map.defineObject('bossKiller'+k, {
                        'type':'dynamic',
                'symbol':'^',
                'color':'blue',
                'interval':100,
                'projectile':true,
                'behavior':function(me){
                if(bosses[k][" _".substr(1,1)+"isDestroyed"]()){
                        me[" _".substr(1,1)+"destroy"]();
                }
               
                var x = bosses[k].getX();
                var y = bosses[k].getY();
                        var dx = Math.abs(me.getX() - x);
                var dy = Math.abs(me.getY() - y);
                var direction = "none";
                if(dx > dy){
                        if(x < me.getX()){
                        direction = 'left';
                    } else {// if (x > me.getX()){
                        direction = 'right';
                    }
                } else {
                        if(y < me.getY()){
                        direction = 'up';
                    } else {// if (y > me.getY()){
                        direction = 'down';
                    }
                }
                me.move(direction);
                },
            'onDestroy':function(me){
                if(bosses[k][" _".substr(1,1)+"isDestroyed"]()) return;
                if(me.getX() != bosses[k].getX()
                        || me.getY() != bosses[k].getY())
                        map.placeObject(me.getX()+Math.floor(Math.random()*2-1)
                        , me.getY(), 'bossKiller'+k);
               
            }
                });
    })(i);
}
 
map.defineObject('helmet', {
        'symbol': 'o',
        'color': 'blue',
        'impassable':true,
});
 
for(var i=0;i<50;i+=1){
        if(i==25) continue;
        map.placeObject(i,map.getPlayer().getY()-2,'helmet');
}
 
map.getPlayer().setPhoneCallback(function(){
 for(var i=0;i<bosses.length;i++){
        map.placeObject(i*2,map.getPlayer().getY()-3,'bossKiller'+i);
 }
});
```

## embix: as redefining his bullets didn't work, I ended up building a launcher

```javascript
    // copied from drone code
    function moveToward(obj, type) {
        var target = obj.findNearest(type);
        var leftDist = obj.getX() - target.x;
        var upDist = obj.getY() - target.y;
        
        var direction;
        if (upDist == 0 && leftDist == 0) {
        	return;
        } if (upDist > 0 && upDist >= leftDist) {
            direction = 'up';
        } else if (upDist < 0 && upDist < leftDist) {
            direction = 'down';
        } else if (leftDist > 0 && leftDist >= upDist) {
            direction = 'left';
        } else {
            direction = 'right';
        }
        
        if (obj.canMove(direction)) {
            obj.move(direction);
        }
    }
	
	map.defineObject('mybullet', {
        'type': 'dynamic',
        'symbol': '*',
        'color': 'yellow',
        'interval': 100,
        'projectile': true,
        'behavior': function (me) {
			moveToward(me, 'boss');
        }
    });
    
	//jQueryObject
    //$('.bullet').removeClass('bullet').addClass('mybullet');
    // redefining seems more complicated after all :-(
    //var dyns = map.getDynamicObjects();
    
    // new dyn objects have to be created after the check
    map.defineObject('launcher', {
        'symbol': 'L',
        'onCollision': function(dyn){
        	map.placeObject(1,5,'mybullet');
        }      
    });
    
    map.placeObject(0, map.getHeight() - 1, 'launcher');
```