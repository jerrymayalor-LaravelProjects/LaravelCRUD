# LaravelCRUD
Laravel CRUD Functions
 
 
Laravel 8 CRUD Tutorial by Example
 
Step 1 – Download Laravel 8 App
    - composer create-project --prefer-dist laravel/laravel LaravelCRUD
    
Step 2 – Setup Database with App
    - dbname: laravel_crud_v1
    - dbusername: root
    
Step 3 – Create Company Model & Migration For CRUD App
    - php artisan make:model Company -m
   
        <!-- Companies table    -->
        public function up()
        {
            Schema::create('companies', function (Blueprint $table) {
                $table->id();
                $table->string('name');
                $table->string('email');
                $table->string('address');
                $table->timestamps();
            })
         }
            
    - php artisan migrate
    
Step 4 – Create Routes
    - Route::resource('companies', CompanyCRUDController::class);
    
Step 5 – Create Company CRUD Controller By Artisan Command
    - php artisan make:controller CompanyCRUDController
        
        <!-- Show all data -->
        public function index()
        {
            $data['companies'] = Company::orderBy('id','desc')->paginate(5);
            return view('companies.index', $data);
        }
  
        <!--   Return view create.blade.php -->
         public function create()
        {
            return view('companies.create');
        }
   
        <!--    Insert data into database -->
        public function store(Request $request)
        {
                $request->validate([
                    'name' => 'required',
                    'email' => 'required',
                    'address' => 'required'
                ]);

                $company = new Company;
                $company->name = $request->name;
                $company->email = $request->email;
                $company->address = $request->address;

                $company->save();

                return redirect()->route('companies.index')->with('success','Company has been created successfully.');
        }

        <!--   Display single data -->
        public function show($id)
        {
            return view('companies.show',compact('company'));
        }
  
        <!--   Return view edit.blade.php -->
        public function edit($id)
        {
            return view('companies.edit',compact('company'));
        }
        
        <!-- Update data into database -->
         public function update(Request $request, $id)
        {
                $request->validate([
                    'name' => 'required',
                    'email' => 'required',
                    'address' => 'required',
                ]);

                $company = Company::find($id);
                $company->name = $request->name;
                $company->email = $request->email;
                $company->address = $request->address;

                $company->save();

                return redirect()->route('companies.index')->with('success','Company Has Been updated successfully');
        }

        <!-- Delete data from  the database -->
        public function destroy($id)
        {
            $company->delete();
                return redirect()->route('companies.index')->with('success','Company has been deleted successfully');  
            }
        }
    
Step 6 – Create Blade Views File
    - Make Directory Name Companies
    - index.blade.php
    - create.blade.php
    - edit.blade.php

Make Directory Name Companies
index.blade.php
create.blade.php
edit.blade.php
Step 7 – Run Laravel CRUD App on Development Server
