<?php
header('Content-Type: application/json; charset=utf-8');

// 1. Capturar código de barras desde ?barcode=
$barcode = isset($_GET['barcode']) ? $_GET['barcode'] : '';

if (!$barcode) {
    echo json_encode(["error" => "No barcode provided"]);
    exit;
}

// 2. URL de la página de precios (El Salvador)
$url = "https://prices.nedostavka.net/es/barcode/$barcode";

// 3. Obtener HTML de esa página
$html = @file_get_contents($url);
if (!$html) {
    echo json_encode(["error" => "No se pudo obtener datos"]);
    exit;
}

// 4. Buscar datos básicos con regex (ejemplo simple, puedes mejorar con DOMDocument)
preg_match('/<title>(.*?)<\/title>/', $html, $titleMatch);
$name = $titleMatch[1] ?? "Producto desconocido";

// Buscar primera imagen del producto
preg_match('/<img[^>]+src="([^"]+)"[^>]*class="[^"]*product[^"]*"/', $html, $imgMatch);
$image = $imgMatch[1] ?? "";

// Buscar precio (puede variar el HTML, aquí un ejemplo genérico)
preg_match('/\$([0-9]+\.[0-9]+)/', $html, $priceMatch);
$price = isset($priceMatch[1]) ? "$" . $priceMatch[1] : "No disponible";

// 5. Devolver JSON
echo json_encode([
    "barcode" => $barcode,
    "name" => $name,
    "image" => $image,
    "price" => $price,
    "source" => $url
], JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
