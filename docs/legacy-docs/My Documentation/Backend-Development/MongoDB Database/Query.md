>[!SUMMARY]- Table of Contents
>- [[Query#Finding Doc|Finding Doc]]
>    - [[Query#find()|find()]]
>    - [[Query#findOne()|findOne()]]
>- [[Query#findById()|findById()]]
>- [[Query#Update|Update]]
>    - [[Query#UpdateOne()|UpdateOne()]]
>    - [[Query#UpdateMany()|UpdateMany()]]
>    - [[Query#findOneAndUpdate()|findOneAndUpdate()]]
>- [[Query#Delete|Delete]]
>    - [[Query#deleteOne()|deleteOne()]]
>    - [[Query#deleteMany()|deleteMany()]]
>- [[Query#General|General]]
>    - [[Query#countDocuments()|countDocuments()]]
>    - [[Query#distinct()|distinct()]]
# Finding Doc

## find()
```javascript
// Find all users with the role 'admin' and select only the username and email fields
const users = await User.find({ role: 'admin' }).select('username email');

// Find all active users created after a certain date, sort them by registration date, and limit the result to 10 users
const users = await User.find({ isActive: true, createdAt: { $gt: new Date('2023-01-01') }})
.sort({ createdAt: -1 })
.limit(10);
```

## findOne()
```javascript
// Find a user by their email address and select all fields except password and refreshToken
const user = await User.findOne({ email: 'example@example.com' }).select('-password -refreshToken');

// Find a user by their email address and populate their associated posts
const userEmail = 'example@example.com';
const user = await User.findOne({ email: userEmail }).populate('posts');
```

# findById()
```javascript
// Find a user by their unique ID and select all fields except password and refreshToken
const userId = 'someUserId';
const user = await User.findById(userId).select('-password -refreshToken');

// Find a user by their unique ID and update their last login timestamp
const userId = 'someUserId';
const updateResult = await User.findByIdAndUpdate(userId, { lastLogin: new Date() }, { new: true });
```

---

# Update

## UpdateOne()
```javascript
// Update the email address of a user with a specific ID
const userId = 'someUserId';
const updateResult = await User.updateOne({ _id: userId }, { email: 'newemail@example.com' });

// Update the email address of a user with a specific ID and create a new document if no match is found
const userId = 'someUserId';
const updateResult = await User.updateOne({ _id: userId }, { email: 'newemail@example.com' }, { upsert: true });
```

## UpdateMany()
```javascript
// Update the status of all orders that are pending to 'completed'
const updateResult = await Order.updateMany({ status: 'pending' }, { status: 'completed' });

// Update the status of all orders that are pending to 'completed' and apply default values if creating new documents
const updateResult = await Order.updateMany({ status: 'pending' }, { status: 'completed' }, { upsert: true, setDefaultsOnInsert: true });
```

## findOneAndUpdate()
```javascript
// Find a user by their email address and update their role to 'admin'
const updatedUser = await User.findOneAndUpdate({ email: 'example@example.com' }, { role: 'admin' }, { new: true });

// Find a user by their email address, update their role to 'admin', and return the updated document
const updatedUser = await User.findOneAndUpdate({ email: 'example@example.com' }, { role: 'admin' }, { new: true });
```

---
# Delete

## deleteOne()
```javascript
// Delete a product with a specific ID
const productId = 'someProductId';
const deleteResult = await Product.deleteOne({ _id: productId });

// Delete a product with a specific ID and set a timeout to avoid waiting for the response indefinitely
const productId = 'someProductId';
const deleteResult = await Product.deleteOne({ _id: productId }).timeout(5000); // Timeout set to 5 seconds
```

## deleteMany()
```javascript
// Delete all users with the role 'guest'
const deleteResult = await User.deleteMany({ role: 'guest' });

// Delete all users with the role 'guest' and specify a write concern
const deleteResult = await User.deleteMany({ role: 'guest' }).writeConcern({ w: 'majority' });
```

---
# General

## countDocuments()
```javascript
// Count the number of users with the role 'admin'
const count = await User.countDocuments({ role: 'admin' });

// Count the number of users with the role 'admin' and specify a maximum execution time
const count = await User.countDocuments({ role: 'admin' }).maxTimeMS(5000); // Maximum execution time set to 5 seconds
```

## distinct()
```javascript
// Get a list of unique categories for all products
const uniqueCategories = await Product.distinct('category');

// Get a list of unique categories for all products and specify a read preference
const uniqueCategories = await Product.distinct('category').read('secondary');
```