# Associative Array and Looping

```
$car = array(
    "brand" => "Ford", 
    "model" => "Mustang",
    "year" => 1964);

foreach ($car as $key => $value) {
  echo "$x: $y <br>";
}
```

# Gets environment variables

```
// Example use of getenv()
$ip = getenv('REMOTE_ADDR');
```

# create and write data to a csv file

```
$data = array("brand" => "Ford", "model" => "Mustang", "year" => 1964); //array
$currentDirectory = __DIR__;
$newPath = '/new-path/'
$csvFileName = 'file-name.csv';
$csvFilePath = $currentDirectory. $newPath . $csvFileName;
$fileHandle = fopen($csvFilePath, 'w');

// Add CSV headers
fputcsv($fileHandle, ['column-1', 'column-2', 'column-3']); # make sure they are real column names

 // Loop through data and write to CSV file
foreach ($data as $row)
{
    fputcsv($fileHandle, $row);
}

fclose($fileHandle);  // Close the file handle

```
