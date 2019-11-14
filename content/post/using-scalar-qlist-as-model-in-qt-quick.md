+++
draft = false
title = "Using scalar QList as model in Qt Quick"
date = "2016-11-27T19:46:12+01:00"
tags = ["qt", "qml", "c++", "programming"]

+++

Today I attempted to expose a QList\<int\> from C++ to QML and use it as model for a 
repeater.

The documentation 
[here](http://doc.qt.io/qt-5/qtqml-cppintegration-data.html#sequence-type-to-javascript-array) 
seems to indicate that a QList\<int\> should be treated as a JS Array in QML.  
This isn't entirely true however, as I found out a QList<int> can not be used as model, 
while a JS Array can.  
You won't get any runtime error, the property will be read but it just won't work.

After some googling I found the following [Stack Overflow 
post](http://stackoverflow.com/questions/31996582/qlistint-cannot-be-used-as-a-model-for-repeater) 
which cleared up a few things for me.  
It also gave me some inspiration for a possible solution.

Since a QList\<int\> is supported by QML, just not as a model in Qt Quick, you can 
query its 
length 
and content just fine.  
Using this knowledge, we can work around the problem by using the fact that numbers can 
be used as models in Qt Quick.

```qml
Repeater
{
	model: myModel.values.length
	
	Text
	{
		text: myModel.values[index]
	}
}
```

Not the prettiest solution, but it works.
