# Create a printable format file all routes.

```
Route::get('/', function () {
    $routeCollection = Route::getRoutes();

    echo "<table style='width:100%'>";
        echo "<tr>";
            echo "<td width='10%'><h4>Name</h4></td>";
            echo "<td width='10%'><h4>URI/h4></td>";
            echo "<td width='70%'><h4>function</h4></td>";
        echo "</tr>";
        foreach ($routeCollection as $value) {
                            echo "<tr>";
                    echo "<td>" . $value->getName() . "</td>";
                    echo "<td>" . $value->uri . "</td>";
                    echo "<td>" . $value->getActionName() . "</td>";
                echo "</tr>";
        }
    echo "</table>";
});
```
