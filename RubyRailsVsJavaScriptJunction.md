# Ruby on Rails Versus JavaScript on Junction Code #

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) | [download](http://code.google.com/p/trimpath/downloads/list) }

Bill Katz notes that the [programming language matters](http://www.billkatz.com/node/42) when creating a framework like [Ruby On Rails](http://www.rubyonrails.com/) (RoR).

For the [TrimPath Junction](http://code.google.com/p/trimpath/wiki/TrimJunction) clone of RoR, the JavaScript language is dynamic enough to support many of the concise concepts of RoR.

JavaScript supports...
  * duck typing
  * closures
  * Lisp-like features
  * metaprogramming
  * eval
  * aspect oriented programming

Here are some code snippet examples for comparison between Rails and Junction.

## Model Example ##
Ruby:
```
class Milestone < ActiveRecord::Base
  belongs_to :creator,  :class_name => 'Person'
  belongs_to :domain,   :class_name => 'Org'
  belongs_to :project
  belongs_to :assignee, :class_name => 'Person'
  has_many :messages
  has_many :todo_lists

  def self.find_recent_by_project(project_id, date)
    Milestone.find('all', :conditions => ["project_id = ? AND created_at > ?", project_id, date]
  end
end
```

JavaScript:
```
Milestone = function() {}

modelInit('Milestone');

Milestone.belongsTo('Creator',  { modelName : 'Person' });
Milestone.belongsTo('Domain',   { modelName : 'Org' });
Milestone.belongsTo('Project');
Milestone.belongsTo('Assignee', { modelName : 'Person' });

Milestone.hasMany('Messages');
Milestone.hasMany('TodoLists');

Milestone.findRecentByProject = function(projectId, date) {
    return Milestone.find('all', { 
        conditions: [ "Milestone.projectId = ? AND Milestone.createdAt > ?,
                      projectId, date ] });
}
```

## Controller Example ##
### Scaffolding ###
Ruby:
```
class TodoListController < ApplicationController
  scaffold :todo_list
end
```

JavaScript:
```
TodoListController = function() {}
scaffold(TodoListController, 'TodoList');
```

### Full Controller ###
Ruby:
```
class MilestoneController < ApplicationController
    layout 'main'

    def index
        @milestones = Milestone.find_all
    end

    def show
        @milestone = Milestone.find @params['id']
    end

    def edit
        @milestone = Milestone.find @params['id']
    end

    def update
        @milestone = Milestone.find @params['id']
        if @milestone.updateAttributes(@params['milestone'])
            flash['notice'] = 'Milestone ' + @milestone.name + ' is updated.'
            redirect_to_action 'show', @milestone.id
        else 
            render_action 'edit'
        end
    end

    def new
        @project = Project.find @params['id']
        if (@project.nil? ||
            (@project.domain &&
             @project.domain != @person.domain)) 
            flash['notice'] = 'Unknown project.'
            return redirect_to_action 'index'
        end

        @milestone = Milestone.new
        @milestone.creator = @person
        @milestone.domain  = @person.domain
        @milestone.project = @project
    end

    def create
        @milestone = Milestone.new @params['milestone']
        @milestone.creator = @person
        @milestone.domain  = @person.domain
        if milestone.save
            flash['notice'] = 'Milestone ' + @milestone.name + ' is created.'
            redirect_to_action 'show', :id => @milestone.id
        else 
            render_action 'new'
        end
    end

    def destroy
        milestone = Milestone.find @params['id']
        if milestone
            milestone.destroy
            flash['notice'] = 'The milestone ' + milestone.name + ' is deleted.';
            redirect_to :controller => 'project', :action => 'show', 
                        :id => milestone.project.id
            return
        end
        flash['notice'] = 'We could not delete an unknown milestone.';
        redirect_to 'home', :action => 'start'
    end
end
```

JavaScript:
```
MilestoneController = function() {
    this.index = function(req, res) {
        res.milestones = Milestone.findAll();
    }

    this.show = function(req, res) {
        res.milestone = Milestone.find(req['objId']);
    }

    this.edit = function(req, res) {
        res.milestone = Milestone.find(req['objId']);
    }

    this.update = function(req, res) {
        with (res) {
            res.milestone = Milestone.find(req['objId']);
            if (milestone.updateAttributes(req['milestone'])) {
                flash['notice'] = 'Milestone ' + milestone.name + ' is updated.';
                redirectToAction('show', milestone.id);
            } else 
                renderAction('edit');
        }
    }

    this.newInstance = function(req, res) {
        with (res) {
            res.project = Project.find(req['objId']);
            if (project == null ||
                (project.domain_id != null &&
                 project.domain_id != person.domain_id)) {
                flash['notice'] = 'Unknown project.';
                return redirectToAction('index');
            }

            res.milestone = Milestone.newInstance();
            milestone.creator_id = person.id;
            milestone.domain_id  = person.domain_id;
            milestone.project_id = project.id;
        }
    }

    this.create = function(req, res) { 
        with (res) {
            res.milestone = Milestone.newInstance(req['milestone']);
            milestone.creator_id = person.id;
            milestone.domain_id  = person.domain_id;
            if (milestone.save()) {
                flash['notice'] = 'Milestone ' + milestone.name + ' is created.';
                redirectToAction('show', milestone.id);
            } else 
                renderAction('newInstance');
        }
    }

    this.destroy = function(req, res) {
        var milestone = Milestone.find(req['objId']);
        if (milestone != null) {
            milestone.destroy();
            res.flash['notice'] = 'The milestone ' + milestone.name + ' is deleted.';
            res.redirectTo('project', 'show', milestone.project_id);
            return;
        }
        res.flash['notice'] = 'We could not delete an unknown milestone.';
        res.redirectTo('home', 'start');
    }
}

MilestoneController.layoutName = 'main';
```

## View Example ##
Ruby ERb template:
```
<div>
    <h2>Milestones</h2>
    <table>
    <% milestones.each do |milestone| %>
      <tr><td><%= link_to milestone.name, :controller => 'milestone', 
                          :action => 'show', :id = milestone.id %></td>
          <td><%= link_to 'Edit', :action => 'edit', :id = milestone.id %></td>
          <td><%= link_to 'Delete', :action => 'destroy', :id =  milestone.id %></td>
          </tr>
    <% end %>
    </table>
</div>
```

[JavaScript Template](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) (JST):
```
<div>
    <h2>Milestones</h2>
    <table>
    {for milestone in milestones}
      <tr><td>${linkTo(milestone.name, 'milestone', 'show', milestone.id)}</td>
          <td>${linkTo('Edit', null, 'edit', milestone.id)}</td>
          <td>${linkTo('Delete', null, 'destroy', milestone.id)}</td>
          </tr>
    {/for}
    </table>
</div>
```

Junction also supports EcmaScript Templates, if you prefer a template markup syntax that looks more like ERb/rhtml or JSP.


---


See also: [Ruby versus JavaScript](http://code.google.com/p/trimpath/wiki/RubyVsJavaScript)


---

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) | [download](http://code.google.com/p/trimpath/downloads/list) }