# Factory Method
**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

![[factory_1.png]]

### When to use factory method?
Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.
#### <span style="color:#bb4646">important</span>
The Factory Method pattern is generally used in the following situations: **A class cannot anticipate the type of objects it needs to create beforehand**. A class requires its subclasses to specify the objects it creates. You want to localize the logic to instantiate a complex object.

##### example:
###### Logistic interface
```typescript
interface ILogistic {
	startTime: Date
	deliveryTime: Date
}
```
###### Logistic ship interface
```typescript
interface IShipLogistic extends ILogistic {
	bayNumber: number
	shipWidth: number;
}
```
###### Logistic Truck interface
```typescript
interface ITruckLogistic extends ILogistic {
	roadNumber: number;
	carType: number;
}
```

##### implementation
an implementation for error handling
```typescript
interface IError {
	statusCode: number
	message: string
	data: TErrorData
}

type TErrorData = {
	level: string
	role: string
	info?: Record<string, unknown>[]
}

type TConstructor<T> = new (...args: any) => T

class AppError implements IError {
	statusCode: number
	message: string
	data: TErrorData

	constructor(error: IError) {
		this.statusCode = error.statusCode
		this.message = error.message
		this.data = error.data
	}

	raiseError() {
		throw {
			statusCode: this.statusCode
			, message: this.message
			, data: this.data
		}
	}
}

class CustomerLevelError extends AppError {

	constructor(error: IError) {
		error.data.level = "CUSTOMER"
		super({ statusCode: error.statusCode, message: error.message, data: error.data })
	}
}

class AdminLevelError extends AppError {

	constructor(error: IError) {
		error.data.level = "ADMIN"
		super({ statusCode: error.statusCode, message: error.message, data: error.data })
	}
}

const errorFactory = <E extends AppError>(ct: TConstructor<E>, ...args: any): E => {
	return new ct(...args)
}


const adminError = errorFactory(AdminLevelError, { statusCode: 404, message: "NotFound!", data: { role: "Super-Admin" } })
const customerError = errorFactory(CustomerLevelError, { statusCode: 404, message: "NotFound!", data: {} })
const simpleError = errorFactory(AppError, { statusCode: 404, message: "NotFound!", data: {} })
adminError.raiseError()
```
