# Polymorphic Association 

1. **Polymorphic Associations**:
	- A polymorphic association allows a model to be associated with multiple other models on a single association. It introduces two columns in the model's table: `*_type` (e.g., `verifiable_type`) and `*_id` (e.g., `verifiable_id`).
	- The `*_type` column stores the class name of the associated model.
	- The `*_id` column stores the primary key value of the associated record.
2. **Purpose of `verifiable_type` in `MobileVerification`**:
    - In your `MobileVerification` model, `verifiable_type` is the attribute used to store the class name of the associated model.
    - This polymorphic association allows a `MobileVerification` instance to be associated with different types of models. The actual association is determined by the value stored in the `verifiable_type` column.
3. **Example Usage**:
    - Let's say you have other models, e.g., `User` and `Admin`, that can be verified via mobile.
    - When creating a `MobileVerification` record, you can set `verifiable_type` to `'User'` or `'Admin'` based on the type of object you want to associate.
    - The corresponding `verifiable_id` would then store the primary key of the associated `User` or `Admin` record.
4. **Associating with Different Models**:
    - This flexibility allows you to associate `MobileVerification` with various models without the need for a separate verification model for each type.
    - For example, you can query all mobile verifications for users (`verifiable_type: 'User'`) or admins (`verifiable_type: 'Admin'`) using the same table and model.


## Delegate 
```ruby 
delegate :currency, to: :store
```

This line means that the current object will delegate the `currency` method to the `store` object. In other words, if you call `currency` on the object where this `delegate` is used, it will be forwarded to the `currency` method of the associated `store` object.