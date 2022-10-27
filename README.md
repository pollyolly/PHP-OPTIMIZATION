## PHP-OPTIMIZATION

### PHP-ARRAY
#### SplFixedArray (FIXED ARRAY SIZE)
```
/* COMMON USE */
$initialMemory = memory_get_usage();
$units = 100000;
$array = []; //USING REGULAR ARRAY
for($i = 0; $i <$units; $i++){
   $array[$i] = $i;
}
$finalMemory = memory_get_usage();
$memoryUsage = ceil(($finalMemory - $initialMemory)/ (1024*1024));
// Memory Usage in MB
echo $memoryUsage;
```
```
/* BETTER USE */
//Using Fixed Array

$initialMemory = memory_get_usage();
$units = 100000;

//Must have the exact size of Array 
//$array can only have integers indexes $array[1] not $array['test']
$array = new SplFixedArray($units); //USING SplFixedArray

for($i = 0; $i <$units; $i++){
   $array[$i] = $i;
}
$finalMemory = memory_get_usage();
$memoryUsage = ceil(($finalMemory - $initialMemory)/ (1024*1024));
// Memory Usage in MB
echo $memoryUsage;
```
#### ARRAY REPETITIVE FUNCTION CALL
```
/* WRONG */
$array[trim($value)] = trim($value);

/* CORRECT */
$normalizedValue = trim($value);
$array[$normalizedValue] = $normalizedValue;

/* WRONG */
$array[$normalizer->normalize($value)] = $normalizer->normalize($value);

/* CORRECT */
$normalizedValue = $normalizer->normalize($value);
$array[$normalizedValue] = $normalizedValue;
```
### PHP 
