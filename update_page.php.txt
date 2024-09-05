<?php include('dbcon.php'); ?>

<?php

if (isset($_GET['id'])) {
    $id = $_GET['id'];

   
    $query = "SELECT * FROM products WHERE id = ?";
    $stmt = mysqli_prepare($connection, $query);
    mysqli_stmt_bind_param($stmt, "i", $id);
    mysqli_stmt_execute($stmt);
    $result = mysqli_stmt_get_result($stmt);

 
    if (!$result) {
        die("Query failed: " . mysqli_error($connection));
    } else {
        $row = mysqli_fetch_assoc($result);
    }
}


if (isset($_POST['update_list'])) {
    if (isset($_GET['id_new'])) {
        $idnew = $_GET['id_new'];
    }

    $name = mysqli_real_escape_string($connection, $_POST['name']);
    $description = mysqli_real_escape_string($connection, $_POST['description']);
    $price = mysqli_real_escape_string($connection, $_POST['price']);
    $quantity = mysqli_real_escape_string($connection, $_POST['quantity']);
    $create = mysqli_real_escape_string($connection, $_POST['create_at']);
    $update = mysqli_real_escape_string($connection, $_POST['update_at']);

   
    $query = "UPDATE products SET name = ?, description = ?, price = ?, quantity = ?, create_at = ?, update_at = ? WHERE id = ?";
    $stmt = mysqli_prepare($connection, $query);
    mysqli_stmt_bind_param($stmt, "ssdisii", $name, $description, $price, $quantity, $create, $update, $idnew);
    $result = mysqli_stmt_execute($stmt);

    if (!$result) {
        die("Query failed: " . mysqli_error($connection));
    } else {
        header('Location: index.php?update_msg=You have successfully updated the data.');
        exit();
    }
}
?>


<form action="update_page_1.php?id_new=<?php echo $id; ?>" method="post">
    <div class="form-group">
        <label for="name">Name</label>
        <input type="text" name="name" value="<?php echo htmlspecialchars($row['name']); ?>" required>
        
        <label for="description">Description</label>
        <input type="text" name="description" value="<?php echo htmlspecialchars($row['description']); ?>" required>
        
        <label for="price">Price</label>
        <input type="number" step="0.01" name="price" value="<?php echo htmlspecialchars($row['price']); ?>" required>
        
        <label for="quantity">Quantity</label>
        <input type="number" name="quantity" value="<?php echo htmlspecialchars($row['quantity']); ?>" required>
        
        <label for="create_at">Created At</label>
        <input type="datetime-local" name="created_at" value="<?php echo htmlspecialchars($row['created_at']); ?>" required>
        
        <label for="update_at">Updated At</label>
        <input type="datetime-local" name="updated_at" value="<?php echo htmlspecialchars($row['updated_at']); ?>" required>
    </div>
    
    <input type="submit" class="btn btn-success" name="update_list" value="UPDATE">
</form>
