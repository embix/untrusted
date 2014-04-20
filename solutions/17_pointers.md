## embix: draw only safe routes
```javascript
		// TODO find a way to remove the API docs
        // wouldn't want the 'good doctor' to find
        // out about map.getCanvasCoords()...
        if(t1.getType() == 'teleporter' &&t2.getType() == 'teleporter'){
            var t1coords=map.getCanvasCoords(t1);
            var t2coords=map.getCanvasCoords(t2);
          
            canvas.beginPath();
            canvas.strokeStyle = 'white';
            canvas.lineWidth = 1;
            canvas.moveTo(t1coords.x, t1coords.y);
            canvas.lineTo(t2coords.x, t2coords.y);
            canvas.stroke();
        }
```