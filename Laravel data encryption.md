# Data Encryption and Decryption in Laravel

This guide explains how to implement encryption and decryption for sensitive data in Laravel models. By following these steps, you can secure data before storing it in the database and decrypt it when retrieving it.

## Prerequisites

- **Laravel**: Ensure you're using a Laravel project.
- **Encryption Key**: Laravel automatically generates an `APP_KEY` in the `.env` file. This key is used by Laravel's encryption service.

## Step 1: Setup Encryption in the Model

In the model, we will use Laravel's `encrypt()` and `decrypt()` functions to handle encryption and decryption automatically for specified fields.

### Doctor Model

Create or update the `Doctor` model with encryption and decryption methods. We will encrypt fields such as `first_name`, `last_name`, `email`, and `mobile` before saving them in the database.

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Support\Facades\Crypt;

class Doctor extends Model
{
    protected $fillable = [
        'first_name', 'last_name', 'email', 'mobile', 'hashed_email', 'password'
    ];

    // Automatically encrypt attributes when setting them
    public function setFirstNameAttribute($value)
    {
        $this->attributes['first_name'] = encrypt($value);
    }

    public function setLastNameAttribute($value)
    {
        $this->attributes['last_name'] = encrypt($value);
    }

    public function setEmailAttribute($value)
    {
        $this->attributes['email'] = encrypt($value);
    }

    public function setMobileAttribute($value)
    {
        $this->attributes['mobile'] = encrypt($value);
    }

    // Automatically decrypt attributes when getting them
    public function getFirstNameAttribute($value)
    {
        return decrypt($value);
    }

    public function getLastNameAttribute($value)
    {
        return decrypt($value);
    }

    public function getEmailAttribute($value)
    {
        return decrypt($value);
    }

    public function getMobileAttribute($value)
    {
        return decrypt($value);
    }
}
```

### Explanation

- **Setter Methods**: Use `set{AttributeName}Attribute()` to encrypt data before it is stored in the database.
- **Getter Methods**: Use `get{AttributeName}Attribute()` to decrypt data when it is retrieved from the database.

## Step 2: Controller for Data Storage and Retrieval

In the controller, you can handle validation and call the model’s encrypted attributes directly without additional encryption/decryption steps.

### DoctorController

The `DoctorController` handles registration by validating input data, encrypting it via the model, and saving it in the database. When fetching the doctor data, it will automatically decrypt the sensitive fields.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Doctor;
use Illuminate\Support\Facades\Hash;

class DoctorController extends Controller
{
    public function register(Request $request)
    {
        // Validate the incoming request
        $validatedData = $request->validate([
            'first_name' => 'required|string|max:255',
            'last_name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:doctors,email',
            'mobile' => 'required|string|size:10|unique:doctors,mobile',
            'password' => 'required|string|min:8|confirmed',
        ]);

        // Hash the email to ensure uniqueness
        $hashedEmail = hash('sha256', $validatedData['email']);

        // Create a new doctor record (model will handle encryption)
        $doctor = Doctor::create([
            'first_name' => $validatedData['first_name'],
            'last_name' => $validatedData['last_name'],
            'email' => $validatedData['email'],
            'hashed_email' => $hashedEmail,
            'mobile' => $validatedData['mobile'],
            'password' => Hash::make($validatedData['password']),
        ]);

        return response()->json([
            'message' => 'Doctor registered successfully',
            'doctor' => $doctor
        ], 201);
    }

    public function show($id)
    {
        // Fetch the doctor record (model will decrypt the data automatically)
        $doctor = Doctor::findOrFail($id);

        return response()->json($doctor);
    }
}
```

### Explanation

- **register Method**: Validates the incoming request, creates a new doctor record, and automatically encrypts fields like `first_name`, `last_name`, `email`, and `mobile` due to the model's encryption methods.
- **show Method**: Retrieves a doctor record by ID. The model's getter methods decrypt the sensitive fields automatically before returning the data.

## Step 3: Database Configuration

Ensure that the `doctors` table columns for sensitive data are of a sufficient length to handle encrypted data (typically, `TEXT` or `LONGTEXT`).

Example migration setup:

```php
Schema::create('doctors', function (Blueprint $table) {
    $table->id();
    $table->text('first_name');
    $table->text('last_name');
    $table->text('email');
    $table->string('hashed_email')->unique(); // SHA-256 hashed email
    $table->text('mobile');
    $table->string('password');
    $table->timestamps();
});
```

> **Note**: Since encrypted values can be significantly longer than plaintext values, `TEXT` is preferred for encrypted fields.

## Step 4: Handling Decryption Exceptions

For enhanced error handling, wrap decryption logic in try-catch blocks in the model’s getters:

```php
public function getFirstNameAttribute($value)
{
    try {
        return decrypt($value);
    } catch (DecryptException $e) {
        return null; // Or handle the error as needed
    }
}
```

## Additional Notes

- **Environment Security**: Ensure the `APP_KEY` is stored securely in the `.env` file. This key is critical for encryption/decryption.
- **Data Backup**: If data integrity is crucial, ensure you have a backup mechanism, as encrypted data cannot be recovered without the correct `APP_KEY`.

## Summary

1. **Model Encryption**: Use setter methods to encrypt data before storing and getter methods to decrypt data upon retrieval.
2. **Controller Logic**: The controller can handle encrypted fields directly without additional encryption code.
3. **Database Configuration**: Use `TEXT` or `LONGTEXT` columns for encrypted fields.
4. **Security Considerations**: Secure your `APP_KEY` and use exception handling in getters for decryption errors.

---

By following this approach, you can easily implement secure encryption and decryption for sensitive data in your Laravel application. Let me know if you have any further questions!