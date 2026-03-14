# Laravel CRUD Operations â€” Complete Guide from Scratch

I'll walk you through everything step by step, using your reference code as the foundation.

---

## ðŸ§  What is Laravel?

Laravel is a **PHP framework** that follows the **MVC pattern**:
- **M**odel â†’ talks to the database
- **V**iew â†’ the HTML/UI the user sees
- **C**ontroller â†’ the brain that connects Model and View

---

## STEP 1 â€” Install Laravel

First, make sure you have **PHP**, **Composer**, and **MySQL** installed.

```bash
# Install Laravel globally
composer global require laravel/installer

# Create a new Laravel project
composer create-project laravel/laravel emp-app

# Go into the project folder
cd emp-app

# Start the development server
php artisan serve
```

Visit `http://127.0.0.1:8000` â€” you'll see the Laravel welcome page.

---

## STEP 2 â€” Create & Connect the Database

**2a. Create a database in MySQL:**
```sql
CREATE DATABASE emp_db;
```

**2b. Open the `.env` file** in your project root and configure:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=emp_db
DB_USERNAME=root
DB_PASSWORD=your_password
```

> `.env` is your environment config file. Laravel reads this to know which database to connect to.

---

## STEP 3 â€” Create the Migration (Database Table)

A **migration** is like a blueprint for your database table â€” written in PHP instead of SQL.

```bash
php artisan make:migration create_emp_table
```

This creates a file inside `database/migrations/`. Open it and write your schema (your reference code):

```php
public function up(): void
{
    Schema::create('emp', function (Blueprint $table) {
        $table->id();                          // Auto-increment primary key
        $table->string("name");               // VARCHAR column
        $table->string("job_title");
        $table->integer("salary");            // INT column
        $table->string("category");
        $table->enum('status', ['1', '2'])->default('1'); // Dropdown-style column
        $table->timestamps();                 // created_at & updated_at
    });
}
```

**Now run the migration** to actually create the table in MySQL:
```bash
php artisan migrate
```

> Think of `migrate` as "execute the blueprint and build the real table."

---

## STEP 4 â€” Create the Model

A **Model** represents a single table in your database. It lets you read/write data using PHP objects instead of raw SQL.

```bash
php artisan make:model Emp
```

Open `app/Models/Emp.php` and configure it:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Emp extends Model
{
    protected $table = "emp";  // Tell Laravel which table this model uses

    // These are the columns that are ALLOWED to be mass-inserted
    // (security feature â€” prevents unwanted columns being saved)
    protected $fillable = ['name', 'job_title', 'salary', 'status'];
}
```

> âš ï¸ **Important:** Your reference code is missing `status` and `category` from `$fillable`. Add them or saving will silently fail for those fields.

---

## STEP 5 â€” Create the Controller

A **Controller** contains all your logic â€” listing, adding, saving, editing, deleting.

```bash
php artisan make:controller EmpController
```

Open `app/Http/Controllers/EmpController.php` and add all methods:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Emp;

class EmpController extends Controller
{
    // READ â€” List all employees (with optional search + pagination)
    public function listEmp(Request $req)
    {
        $name = $req->name;              // Get search input from URL ?name=John
        $data = Emp::select('*');        // Start building the query

        if ($name != '') {
            $data->where("name", $name); // Add WHERE clause only if searching
        }

        $data = $data->paginate(4);      // Show 4 records per page

        return view('emp_list', compact('data')); // Send $data to the view
    }

    // Show the ADD form
    public function addEmp()
    {
        return view('emp_add');          // Just load the blank form page
    }

    // CREATE â€” Save new employee to DB
    public function saveEmp(Request $req)
    {
        // Validate first â€” stop if rules fail
        $req->validate([
            'name'   => 'required|min:3|max:15|alpha',
            'salary' => 'required|min:100|numeric'
        ]);

        // Insert into database
        Emp::create([
            "name"      => $req->name,
            "job_title" => $req->job_title,
            "salary"    => $req->salary,
            "status"    => $req->status
        ]);

        return redirect('emp_list');     // Go back to list after saving
    }

    // DELETE â€” Remove an employee by ID
    public function deleteEmp($id)
    {
        Emp::where('id', $id)->delete(); // Find by ID and delete
        return redirect('emp_list');
    }

    // Show EDIT form (load existing data)
    public function editEmp(Request $req, $id)
    {
        $emp = Emp::find($id);           // Fetch the single record
        return view("edit_emp", compact('emp')); // Send it to the edit form
    }

    // UPDATE â€” Save edited data
    public function updateEmp(Request $req, $id)
    {
        Emp::where('id', $id)->update([
            "name"      => $req->name,
            "job_title" => $req->job_title,
            "salary"    => $req->salary,
            "status"    => $req->status
        ]);
        return redirect('emp_list');
    }
}
```

---

## STEP 6 â€” Define Routes

Open `routes/web.php` â€” this is where you map URLs to controller methods:

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\EmpController;

Route::get('emp_list',         [EmpController::class, 'listEmp']);   // READ
Route::get('emp_add',          [EmpController::class, 'addEmp']);    // Show add form
Route::post('emp_insert',      [EmpController::class, 'saveEmp']);   // CREATE
Route::get('delete_emp/{id}',  [EmpController::class, 'deleteEmp']); // DELETE
Route::get('edit_emp/{id}',    [EmpController::class, 'editEmp']);   // Show edit form
Route::post('update_emp/{id}', [EmpController::class, 'updateEmp']); // UPDATE
```

> `{id}` is a **dynamic segment** â€” it captures the number from the URL like `/delete_emp/5`

---

## STEP 7 â€” Create the Views (Blade Templates)

Laravel uses **Blade** â€” HTML files with special `{{ }}` and `@` syntax.

**`resources/views/emp_list.blade.php`** â€” List page:
```html
<!DOCTYPE html>
<html>
<body>

<a href="/emp_add">Add Employee</a>

<!-- Search Form -->
<form method="GET" action="/emp_list">
    <input type="text" name="name" placeholder="Search by name">
    <button type="submit">Search</button>
</form>

<!-- Employee Table -->
<table border="1">
    <tr>
        <th>ID</th><th>Name</th><th>Job Title</th>
        <th>Salary</th><th>Status</th><th>Actions</th>
    </tr>

    @foreach($data as $emp)
    <tr>
        <td>{{ $emp->id }}</td>
        <td>{{ $emp->name }}</td>
        <td>{{ $emp->job_title }}</td>
        <td>{{ $emp->salary }}</td>
        <td>{{ $emp->status == 1 ? 'Active' : 'Inactive' }}</td>
        <td>
            <a href="/edit_emp/{{ $emp->id }}">Edit</a>
            <a href="/delete_emp/{{ $emp->id }}"
               onclick="return confirm('Delete this record?')">Delete</a>
        </td>
    </tr>
    @endforeach
</table>

<!-- Pagination Links -->
{{ $data->links() }}

</body>
</html>
```

**`resources/views/emp_add.blade.php`** â€” Add form:
```html
<!DOCTYPE html>
<html>
<body>

<h2>Add Employee</h2>

<!-- Show validation errors -->
@if($errors->any())
    <ul style="color:red">
        @foreach($errors->all() as $error)
            <li>{{ $error }}</li>
        @endforeach
    </ul>
@endif

<form method="POST" action="/emp_insert">
    @csrf  {{-- Laravel security token, ALWAYS required on POST forms --}}

    Name:      <input type="text" name="name"><br>
    Job Title: <input type="text" name="job_title"><br>
    Salary:    <input type="number" name="salary"><br>
    Status:
    <select name="status">
        <option value="1">Active</option>
        <option value="2">Inactive</option>
    </select><br>

    <button type="submit">Save</button>
</form>

</body>
</html>
```

**`resources/views/edit_emp.blade.php`** â€” Edit form:
```html
<!DOCTYPE html>
<html>
<body>

<h2>Edit Employee</h2>

<form method="POST" action="/update_emp/{{ $emp->id }}">
    @csrf

    Name:      <input type="text" name="name" value="{{ $emp->name }}"><br>
    Job Title: <input type="text" name="job_title" value="{{ $emp->job_title }}"><br>
    Salary:    <input type="number" name="salary" value="{{ $emp->salary }}"><br>
    Status:
    <select name="status">
        <option value="1" {{ $emp->status == 1 ? 'selected' : '' }}>Active</option>
        <option value="2" {{ $emp->status == 2 ? 'selected' : '' }}>Inactive</option>
    </select><br>

    <button type="submit">Update</button>
</form>

</body>
</html>
```

---

## ðŸ—ºï¸ Full Flow Summary

```
Browser URL          HTTP Method    Route              Controller Method    Action
â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
/emp_list            GET         â†’ listEmp()        â†’ Fetch all & show list
/emp_add             GET         â†’ addEmp()         â†’ Show blank add form
/emp_insert          POST        â†’ saveEmp()        â†’ Validate & insert to DB
/edit_emp/{id}       GET         â†’ editEmp()        â†’ Load record & show edit form
/update_emp/{id}     POST        â†’ updateEmp()      â†’ Update record in DB
/delete_emp/{id}     GET         â†’ deleteEmp()      â†’ Delete record from DB
```

---

## âš ï¸ Key Issues to Fix in Your Reference Code

| Issue | Location | Fix |
|---|---|---|
| `$fillable` missing `status` & `category` | `Emp.php` | Add both to the array |
| `editEmp()` doesn't fetch the record | `EmpController.php` | Add `$id` parameter and `Emp::find($id)` |
| No `updateEmp()` route/method | `web.php` + Controller | Add both as shown above |
| `@csrf` missing (assumed) | Blade forms | Always include on POST forms |
