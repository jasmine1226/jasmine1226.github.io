---
layout: post
title:      "Handling Many-to-Many Relationships in React/Redux/Rails API Web App"
date:       2020-03-19 00:25:14 -0400
permalink:  handling_many-to-many_relationships_in_react_redux_rails_api_web_app
---

The example I'm using here is a learning site with many courses and many career paths. A career path can have many courses, and a course can be added to many career paths. The app has the following features:
* Fetch and display all career paths and courses
* Display all courses associated with one career path under the career path show page
* Add a course and delete a course from a career path. This should remove the association without deleting the course from the database.

There are plenty of examples out there with has_many & belongs_to relationships, and it took me some time to remember/integrate what I had learned working with Rails and figure out how to wire it. You can see the rest of my codes on my GitHub repo: https://github.com/jasmine1226/career-path

I guess after all the struggle, my main takeaways are:
* Set up correspondant routes and actions to handle adding and deleting many-to-many relationships.
* Pass down course_id as a param in your custom routes.
* Make sure your fetch request uses the custom routes you created.
* As a Rails recap, to add many-to-many association, you can do "career_path.courses.push(course)"
* To delete a many-to-many association, you can do "career_path.courses.delete(course)"

0. Create Rails API and React app, and set up redux

You can reference this helpful article for details setup instructions: https://medium.com/@christine_tran/portfolio-project-5-react-redux-rails-api-selfcare-42aa58311d3f

1. Set up many-to-many relationship through joint table CareerPathCourse

// models/Course.rb
```
class Course < ApplicationRecord
  has_many :career_path_courses
  has_many :career_paths, through: :career_path_courses
end
```
// models/CareerPath.rb
```
class CareerPath < ApplicationRecord
  has_many :career_path_courses
  has_many :courses, through: :career_path_courses
  accepts_nested_attributes_for :courses
end
```
// models/CareerPathCourse.rb
```
class CareerPathCourse < ApplicationRecord
  belongs_to :career_path
  belongs_to :course
end
```
Since I'm planning to display, add and delete courses associated with one career path,  I only added "accepts_nested_attributes_for :courses" for career

2. Include career_path.courses when rendering json data.

// career_path_controller.rb
```
def index
      @career_paths = CareerPath.all
      
      render json: @career_paths.to_json(:include => {
        :courses => {:only => [:id, :title, :url, :length]}})
end
```
Do this for other controller actions as well.

3. Add custom conroller actions to add and delete a course

// career_path_controller.rb
  ```
	def add_course
		@career_path = CareerPath.find(params[:id])
		if params[:course_id]
			course = Course.find(params[:course_id])
			@career_path.courses.push(course)
			render json: @career_path.to_json(:include => {
				:courses => {:only => [:id, :title, :url, :length]}}), status: 200
		else
			render json: @career_path.errors, status: :unprocessable_entity
		end
	end

   
def delete_course
      @career_path = CareerPath.find(params[:id])
      course = Course.find(params[:course_id])
      if course
        @career_path.courses.delete(course)
      end
    end  ```
		
4. Add custom routes mapping to the custom actions

// config/routes.rb
  ```
      resources :career_paths do
          collection do
            patch ':id/courses/:course_id', action: :add_course
            delete ':id/courses/:course_id', action: :delete_course
          end
      end
		  ```
		There are other ways to do this, but I wanted to the routes to be "career_paths/career_path_id/courses/course_id" so I decided to go with this hard-coded routes. You can also use "member" instead of "collection" but it will give you slightly different results.

5. Delete a course from career path

// components/courses/CourseTable.js  >> or wherever you have your course delete button
  ```
const handleDelete = event => {
    console.log(event.target.dataset.id);
    fetch(
      `http://localhost:3000/api/v1/career_paths/${props.careerPathId}/courses/${event.target.dataset.id}`,
      {
        method: "DELETE"
      }
    ).then(res => {
      alert("The course has been removed from the career path.");
      props.fetchCareerPaths();
    });
  };
  
<Button variant="primary" data-id={course.id} onClick={event => handleDelete(event)} > Delete </Button>
```
Add handleDelete event and add it to the button. Make sure your fetch URL matches the custom routes you made in step 4.
One thing to note is, I added "props.fetchCareerPaths()" after the deletion alert so that the page will refresh and diplay the updated data. This works for both add a course and delete a course.


6. Add a course to career path

//  actions/careerPathActions.js
  ```
export const editCareerPath = (careerPath, courseId) => {
  return dispatch => {
    return fetch(
      `http://localhost:3000/api/v1/career_paths/${careerPath.id}/courses/${courseId}`,
      {
        method: "PATCH"
      }
    )
      .then(res => res.json())
      .then(careerPath => {
        dispatch({ type: "EDIT_CAREER_PATH", careerPath });
      })
      .catch(error => console.log(error));
  };
};
  ```
// reducers/CareerPathReducers.js
  ```
  case "EDIT_CAREER_PATH":
      let editedCareerPaths = state.careerPaths.map(careerPath => {
        if (careerPath.id === action.careerPath.id) {
          return action.careerPath;
        } else {
          return careerPath;
        }
      });
  ```
I imported the action into my CareerPathContainer and passed it into editCareerPath page as a prop.

// containers/CareerPathContainers.js
  ```
import React, { Component } from "react";
import CareerPaths from "../components/career-paths/CareerPaths";
import { connect } from "react-redux";
import {
  fetchCareerPaths,
  createCareerPath,
  editCareerPath
} from "../actions/careerPathActions";

class CareerPathContainer extends Component {
  componentDidMount() {
    this.props.fetchCareerPaths();
  }

  render() {
    const {
      careerPaths,
      courses,
      createCareerPath,
      editCareerPath,
      fetchCareerPaths
    } = this.props;

    return (
      <div>
        {careerPaths === [] ? null : (
          <CareerPaths
            careerPaths={careerPaths}
            courses={courses}
            createCareerPath={createCareerPath}
            editCareerPath={editCareerPath}
            fetchCareerPaths={fetchCareerPaths}
          />
        )}
      </div>
    );
  }
}

const mapStateToProps = state => ({
  careerPaths: state.careerPaths,
  courses: state.courses
});

const mapDispatchToProps = dispatch => {
  return {
    fetchCareerPaths: () => dispatch(fetchCareerPaths()),
    createCareerPath: careerPath => dispatch(createCareerPath(careerPath)),
    editCareerPath: (careerPath, courseId) =>
      dispatch(editCareerPath(careerPath, courseId))
  };
};

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(CareerPathContainer);
  ```
And I call the props editCareerPath on form submission.

// component/editCareerPaths.js
  ```
handleOnSubmit = event => {
    event.preventDefault();
    this.props.editCareerPath(this.props.careerPath, this.state.courseId);
    this.setState({
      courseId: null
    });
  };
	
	<Form onSubmit={event => this.handleOnSubmit(event)}>
	  // form content
  </Form>
  ```
