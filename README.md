<?php
// Conexão com o banco de dados
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "lost_and_found";

// Criar a conexão
$conn = new mysqli($servername, $username, $password, $dbname);

// Verificar a conexão
if ($conn->connect_error) {
    die("Conexão falhou: " . $conn->connect_error);
}

// Registrar um item achado
if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_POST['register_found'])) {
    $finderName = $_POST['finder_name'];
    $location = $_POST['location'];
    $date = $_POST['date'];
    $description = $_POST['description'];

    $stmt = $conn->prepare("INSERT INTO found_items (finder_name, location, date, description) VALUES (?, ?, ?, ?)");
    $stmt->bind_param("ssss", $finderName, $location, $date, $description);
    $stmt->execute();
    echo "Item achado registrado com sucesso!";
    $stmt->close();
}

// Listar itens achados
$result = $conn->query("SELECT * FROM found_items");
if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        echo "<p>Item: " . $row["description"] . " | Local: " . $row["location"] . " | Data: " . $row["date"] . " | Encontrado por: " . $row["finder_name"] . "</p>";
    }
} else {
    echo "Nenhum item achado registrado.";
}

// Fechar a conexão
$conn->close();
?>
