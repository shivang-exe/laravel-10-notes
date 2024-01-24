##                               Upload Files in Laravel

Go to your config/filesystem.php, the file tell us about the sys configuration of disk, if we accept any data from server where should we store it : 'local', 'local+public' , 'local+private' , 's3' (AWS)


For all of these configurations are already pre-written.
But for 'local+public' : means anyone from public can access the assets inside this folder which is inside another folder which is not allowed to access publicly. How? through a soft.

A soft link has been created from storage/app/public to another folder where users can only get access to public folder items.

{{

        // get the access of uploaded file
        $file = $request->file('name');                 
        // uploaded file will be stored in private directory inside cvs and path is returned
        $path = $file->store('cvs', 'private' );   


        $job->jobApplications()->create([
            'user_id' => auth()->user()->id,
            'expected_salary' => $validated['expected_salary'],
            'cv_path' => $path
        ]);


}}