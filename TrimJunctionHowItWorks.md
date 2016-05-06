# How Does Junction Work #

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) }

Junction decorates your functions and prototypes dynamically.  It's like aspect-oriented-programming.  The decorating happens at the moment when you give Junction any meta information or declarations about your application.

For example, let's take a look at the code for a sample Milestone model object, which could be part of a Project-Milestone tracking application...
```
function Milestone() {}

modelInit('Milestone');

Milestone.belongsTo('Creator',  { modelName : 'Person' });
Milestone.belongsTo('Domain',   { modelName : 'Org' });
Milestone.belongsTo('Project');
Milestone.belongsTo('Assignee', { modelName : 'Person' });

Milestone.hasMany('Messages');
Milestone.hasMany('TodoLists');

Milestone.findRecentByProject = function(projectId, date) {
    return Milestone.find('all', {
      conditions: [ "Milestone.project_id = ? AND Milestone.created_at > ?",
                    projectId, date ] });
}
```

When '''modelInit()''' is invoked, Junction knows that the Milestone function is to be a model object in the Model-View-Controller sense.  The '''modelInit()''' function performs the following...
  * It associates the model name string 'Milestone' with the Milestone function.
  * It assumes the plural name is 'Milestone' + 's', or 'Milestones'.
    * This is overridable using the pluralName() function.
  * It assumes the database table name is 'Milestone'.
    * This is overridable using the tableName() function.
  * It adds some functions or methods to the Milestone function object, such as...
    * Milestone.find()
    * Milestone.findActive()
    * Milestone.findAll()
    * Milestone.findFirst()
    * Milestone.findBySql()
    * Milestone.newInstance()
      * You should use "m = Milestone.newInstance()" instead of "m = new Milestone()" when creating instances of the Milestone.
  * It also adds some functions or methods to the Milestone.prototype object, to give each Milestone instance object extra functionality, such as...
    * save()
    * updateAttributes()
    * destroy()
    * For example, you can now invoke "m.save(); m.destroy();".

Back to the sample Milestone code, when the '''belongsTo()''' declaration function is called, it adds even more functions to the Milestone's prototype object.  For example, when ''belongsTo('Creator', { modelName : 'Person' })'' is invoked, the following happens...
  * It adds a getCreator() function or method to Milestone.prototype.
  * It remembers that the associated model object is the function named 'Person', instead of the normally guessed default name which would have been a function named 'Creator'.
  * The options can be set through the second paramater hash object to '''belongsTo()''', includes...
    * modelName
    * foreignKey

Similarly, when the '''hasMany()''' declaration function is called, such as ''hasMany('Messages')'', Junction adds methods to the Milestone.prototype object such as getMessages().


---

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) }