# README

Rails Pods API - Employees CRUD:

1. Add an employees resource in the routes.rb file like so:

Rails.application.routes.draw do
  resources :employees
end

Running rails routes, will now show that we have defined routes for the employees resource, using all the standard RESTful actions, such as index, create, show, update and destroy.

2. Now, we need to define an Employees controller class that will contain the methods for our RESTful actions.

Run rails g controller Employees

3. Open app/controllers/employees_controller.rb and inside the EmployeesController class, define the methods for the RESTful actions, like so:

class EmployeesController < ApplicationController
  def index
    employees = Employee.order("created_at DESC")
    render json: { status: "SUCCESS", message: "Loaded employees", data: employees }, status: :ok
  end

  def show
    employee = Employee.find(params[:id])
    render json: { status: "SUCCESS", message: "Loaded employee", data: employee }, status: :ok
  end

  def create
    employee = Employee.new(employee_params)

    if employee.save
      render json: { status: "SUCCESS", message: "Saved employee", data: employee }, status: :ok
    else
      render json: { status: "ERROR", message: "employee not saved", data: employee.errors }, status: :unprocessable_entity
    end
  end

  def update
    employee = Employee.find(params[:id])
    if employee.update_attributes(employee_params)
      render json: { status: "SUCCESS", message: "Updated employee", data: employee }, status: :ok
    else
      render json: { status: "ERROR", message: "employee not updated", data: employee.errors }, status: :unprocessable_entity
    end
  end

  def destroy
    employee = Employee.find(params[:id])
    employee.destroy
    render json: { status: "SUCCESS", message: "Deleted employee", data: employee }, status: :ok
  end

  private

  def employee_params
    params.permit(:first_name, :last_name, :google_user_id)
  end
end
