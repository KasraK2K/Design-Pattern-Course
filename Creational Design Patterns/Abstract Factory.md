# Abstract Factory 

---

**Abstract Factory** patterns work around a super-factory which creates other factories.

![[abstract_factory_1.png]]

### When to use factory method?
 Use the Abstract Factory when your code needs to work with various families of related products


##### Implementation (Javascript)
```javascript
class Painter {
	draw() {}
}

class RedCircle extends Painter {
	draw() {
		return '🔴'
	}
}

class BlueCircle extends Painter {
	draw() {
		return '🔵'
	}
}
 
class RedSquare extends Painter {
	draw() {
		return '🟥'
	}
}

class BlueSquare extends Painter {
	draw() {
		return '🟦'
	}
}

abstract class AbstractFactory {
	abstract createShape(color);
}

class CircleFactory extends AbstractFactory {
	createShape(color){
		let shape;
		if (shapeName === 'red') shape = new RedCircle().draw();
		else if (shapeName === 'square') shape = new BlueCircle().draw();
		console.log(shape);
	}
}

class SquareFactory extends AbstractFactory {
	createShape(color){
		let shape;
		if (shapeName === 'red') shape = new RedSquare().draw();
		else if (shapeName === 'square') shape = new BlueSquare().draw();
		console.log(shape);
	}
}

const factoryPainter = (shape, color)=>{
	if(shape=='circle')	return new CircleFactory().createShape(color);
	else if (shape=='square') return new SquareFactory().createShape(color);
}

factoryPainter('circle', 'red');
```

