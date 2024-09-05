<?php  
include 'dbcon.php';

if(isset($_POST['insert'])){
    $name = $_POST['name'];
    $des = $_POST['description'];
    $price = $_POST['price'];
    $quan = $_POST['quantity'];
    $create = $_POST['create'];
    $update = $_POST['update'];

    if($name == "" || empty($name)){
        header('location:index.php?message=You need to fill in the last name');
    }

    else{
        $query = "insert into products (`name`, `description`, `price`, `quantity`, `created_at`, `updated_at`) values('$name', '$des', '$price', '$quan', '$create', '$update')";
        $result = mysqli_query( $connection,$query);

        if(!$result){
            die("Query Failed".mysqli_error($connection));
        }
        else{
            header('location:index.php?insert_msg=Your data has been added successfully');
        }
    }

    
}


?>