# Laravel *HasMany* have not sync method.

The laravel have not sync method in *HasMany* relationship, so we planned to create a macro in *HasMany* to achieve the sync method.

Paste this code in Service provider and use HasMany relation in sync function.

```

    ...
    public function boot()
    {
       ...
        HasMany::macro('sync', function (array $requestArray, $includes = [], $excludes=[],  $deleting = true) {  
            // Get the current key values from database.
            $serverArray = $this->newQuery()->select($includes)->get()->toArray();

            $requestData =[];
            foreach($requestArray as $index => $request){
                foreach($excludes as $e){
                    //remove the exclude index
                    unset($request[$e]);
                }
                //convert casting
                $requestData[$index]=array_map('strval', $request);
            }
            $serverData=[];
            foreach($serverArray as $index => $request){
                //convert casting
                $serverData[$index] = array_map('strval', $request);
            }
            
            // Compare all values by a json_encode
            $diff = array_diff(array_map('json_encode', $serverData), array_map('json_encode', $requestData));
            // Json decode the result
            $deleteRecords = array_map('json_decode', $diff);
            
            // Compare all values by a json_encode
            $diff = array_diff(array_map('json_encode', $requestData), array_map('json_encode', $serverData));
            // Json decode the result
            $insertRecords = array_map('json_decode', $diff);

            //remove the unwanted records
            $delete = [];
            if($deleting == true){
                foreach($deleteRecords as $record){
                    $record = (array) $record;
                    if(is_array($record)){
                        $deleteRecords = $this->getRelated()->where($record)->get();
                        foreach($deleteRecords as $deleteRecord){
                            $delete[]=$deleteRecord;
                            $deleteRecord->delete();
                        }
                    }
                }
            }
            
            //insert the finding new records
            $new = [];
            foreach($insertRecords as $record){
                $record = (array) $record;
                if(array($record)){
                     $new[] = $this->create($record);
                }
            }
            return ['created' => $new, 'deleted' => $delete ];
        });
        ...
    }
    ...
```
