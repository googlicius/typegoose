---
title: "Quick Start Guide"
---

<sub>Please use [Dark-Reader](https://chrome.google.com/webstore/detail/dark-reader/eimadpbcbfnmbkopoojfekhnkhdbieeh) for a dark version of the site</sub>

## Quick Overview of Typegoose

Typegoose is an "wrapper" for mongoose models

Instead of:

```ts
interface Car {
  model?: string;
}

interface Job {
  title?: string;
  position?: string;
}

interface User {
  name?: string;
  age!: number;
  job?: Job;
  car?: Car | string;
}

const CarModel = mongoose.model('Car', {
  model: string,
});

const UserModel = mongoose.model('User', {
  name: String,
  age: { type: Number, required: true },
  job: {
    title: String;
    position: String;
  },
  car: { type: Schema.Types.ObjectId, ref: 'Car' }
});
```

You can just:

```ts
class Job {
  @prop()
  public title?: string;

  @prop()
  public position?: string;
}

class Car {
  @prop()
  public model?: string;
}

class User {
  @prop()
  public name?: string;

  @prop({ required: true })
  public age!: number;

  @prop()
  public job?: Job;

  @prop({ ref: Car })
  public car?: Ref<Car>;
}
```

## How to Start using it

*Please note that this guide is for Typegoose 7.0.0*

### Requirements

- TypeScript 3.9+ (sicne 7.1)
- NodeJS 10.15.0+
- Mongoose 5.9.13+
- An IDE that supports TypeScript linting (VSCode is recommendet)
- This Guide expect you to know how mongoose (at least its models) work

### How to Use

Lets say you have an mongoose model like:

```ts
const kittenSchema = new mongoose.Schema({
  name: String
});

const Kitten = mongoose.model('Kitten', kittenSchema);

let document = await Kitten.create({ name: 'Kitty' });
// "document" has no types
```

with typegoose it can be converted to something like:

```ts
class KittenClass {
  @prop()
  public name: string
}

const Kitten = getModelForClass(KittenClass);

let document = await Kitten.create({ name: 'Kitty' });
// "document" has proper types of KittenClass
```

Please note that `new Kitten({})` & `Kitten.create({})` has no types of KittenClass, because typegoose doesn't modify functions of mongoose

## Do's and Dont's of Typegoose

- Typegoose is a wrapper for mongoose's models
- Typegoose aims to not modify any functions of mongoose
- Typegoose aims to get mongoose's models to be stable through type-information
- Typegoose aims to make mongoose more usable by making the models more type-rich with TypeScript
- Decorated schema configuration classes (like KittenClass above) must use explicit type declaration
