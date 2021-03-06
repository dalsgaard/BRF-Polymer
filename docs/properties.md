# Properties

Properties can be declared in the `properties` property.

In the simple syntax only the name and the type is declared.

```
Polymer({
  is: 'foo-element',
  properties: {
  	bar: String,
  	baz: Number
  }
});
```

The _type_ is one of the following `Boolean`, `Date`, `Number`, `String`, `Array` or `Object`.

Properties can be initialised through attributes.

```
Polymer({
  is: 'foo-element',
  properties: {
  	bar: String,
  	baz: Number
  },
  ready: function () {
  	console.log(this.bar, this.baz);
  }
});
```

```
<foo-element bar="1" baz="2"></foo-element>
```

## Configuration Object

In addition to the simple syntax properties can be configured via an object. This object can have the following keys.

### type

This key corresponds to the value in the simple syntax.

```
properties: {
  bar: {
  	type: String
  }
}
```

This key is mandatory.

### value

The default value for the property. The value for this key can either be a boolean, number, string or function. If the value is a function the return value vil be used as the default value.

```
properties: {
  bar: {
  	type: String,
  	value: 'bar'
  },
  baz: {
  	type: Number,
  	value: function () {
  	  return 3;
  	}
  }
}
```

### reflectToAttribute

Boolean value determining whether or not the corresponding attribute value should reflect this property.

```
properties: {
  bar: {
  	type: String,
  	reflectToAttribute: true
  }
}
```

### readOnly

Boolean value determining whether or not this property is read-only.

```
properties: {
  bar: {
  	type: String,
  	readOnly: true
  }
}
```

The read-only can be changed by calling the `_set<PropertyName>` method on the element. In this case the `_setBar` method.

```
ready: function () {
  this._setBar(5);
}
```

### notify

Boolean value determining whether or not the property can be used by the two-way data binding system. 

```
properties: {
  barBaz: {
  	type: String,
  	notify: true
  }
}
```

If notify is true an `<property-name>-changed` event will be fired when the attribute is changed. In this case a `bar-baz-changed` event.

```
foo.addEventListener('bar-baz-changed', function (e) {
  console.log(e);
});
```

### computed

String value interpreted as a method name and an argument list. The method is called to calculate the value of the property. If one or more of the arguments from the argument list changes, the property value is recalculated.

```
Polymer({
  is: 'foo-element',
  properties: {
  	firstName: String,
  	latName: String,
  	fullName: {
  	  type: String,
  	  computed: 'computeFullName(firstName, lastName)'
	}
  },
  computeFullName: function (firstName, lastName) {
  	return this.firstName + ' ' + this.lastName;
  }
});
```

The computed key can be used together with the notify key.

```
properties: {
  firstName: String,
  latName: String,
  fullName: {
    type: String,
  	computed: 'computeFullName(firstName, lastName)',
  	notify: true
  }
}
```

### observer

String value interpreted as a method name. The method is called whenever the value of the property changes.

```
Polymer({
  is: 'foo-element',
  properties: {
  	bar: {
  	  type: Number,
  	  observer: 'barChanged'
	}
  },
  barChanged: function (newValue, oldValue) {
  	console.log('From ' + oldValue + ' to ' + newValue);
  }
});
```


## Observers

The `observers` array can be used to listen to multiple properties or sub-paths of a property.

### Observing multiple properties

This syntax is very much like computed properties - a string value interpreted as a method name and an argument list. The listener is called when one or more of the arguments from the argument list is changed, and none of the arguments are undefined.

```
Polymer({
  is: 'foo-element',
  properties: {
    bar: String,
    baz: String
  },
  observers: [
    'barOrBazChanged(bar, baz)'
  ],
  barOrBazChanged: function (bar, baz) {
    console.log(bar, baz);
  }
});
```

### Observing property sub-paths

A 'dot' notation is used to observe a sub-path.

```
Polymer({
  is: 'foo-element',
  properties: {
    user: {
      type: Object,
      value: function () {
        return {};
      }
    }
  },
  observers: [
    'emailChanged(user.email)'
  ],
  emailChanged: function (user) {
    console.log(user);
  }
});
```

The listener is called when the sub-path changes, but only is `set` method is used (or notifyPath).

```
foo.set('user.email', 'kim@kimdalsgaard.com');
```
