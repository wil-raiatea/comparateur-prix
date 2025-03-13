import React, { useState } from "react";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { BarcodeScanner } from "@/components/ui/barcode-scanner";

const mockData = [
  { store: "Magasin A", product: "Lait", price: "200 XPF", barcode: "123456" },
  { store: "Magasin B", product: "Lait", price: "180 XPF", barcode: "123456" },
  { store: "Magasin C", product: "Lait", price: "210 XPF", barcode: "123456" },
];

export default function PriceComparator() {
  const [search, setSearch] = useState("");
  const [results, setResults] = useState([]);
  const [scannedCode, setScannedCode] = useState("");

  const handleSearch = () => {
    const filtered = mockData.filter((item) =>
      item.product.toLowerCase().includes(search.toLowerCase()) ||
      item.barcode === scannedCode
    );
    setResults(filtered);
  };

  const handleScan = (code) => {
    setScannedCode(code);
    setSearch(""); // Reset text search when scanning
    handleSearch();
  };

  return (
    <div className="p-4 max-w-lg mx-auto">
      <h1 className="text-xl font-bold mb-4">Comparateur de Prix</h1>
      <div className="flex gap-2 mb-4">
        <Input
          placeholder="Rechercher un produit..."
          value={search}
          onChange={(e) => setSearch(e.target.value)}
        />
        <Button onClick={handleSearch}>Rechercher</Button>
      </div>
      <div className="mb-4">
        <BarcodeScanner onScan={handleScan} />
      </div>
      <div>
        {results.map((item, index) => (
          <Card key={index} className="mb-2">
            <CardContent className="p-4">
              <p className="font-semibold">{item.product}</p>
              <p>{item.store} - {item.price}</p>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}
