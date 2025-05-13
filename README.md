import React, { useEffect, useState } from 'react';
import axios from 'axios';

function App() {
  const [products, setProducts] = useState([]);
  const [form, setForm] = useState({
    name: '',
    description: '',
    pricePerUnit: '',
    minOrderQty: '',
    imageUrl: ''
  });

  useEffect(() => {
    axios.get('http://localhost:5000/products')
      .then(res => setProducts(res.data));
  }, []);

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    axios.post('http://localhost:5000/products', form)
      .then(res => setProducts([...products, res.data]));
  };

  return (
    <div className="App" style={{ padding: 20 }}>
      <h1>Wholesale Product List</h1>
      <div style={{ display: 'grid', gridTemplateColumns: 'repeat(auto-fill, minmax(250px, 1fr))', gap: 20 }}>
        {products.map(p => (
          <div key={p._id} style={{ border: '1px solid #ccc', padding: 10 }}>
            <img src={p.imageUrl} alt={p.name} style={{ width: '100%' }} />
            <h3>{p.name}</h3>
            <p>{p.description}</p>
            <p><strong>${p.pricePerUnit} per unit</strong></p>
            <p>Min order: {p.minOrderQty}</p>
          </div>
        ))}
      </div>

      <h2>Add New Product</h2>
      <form onSubmit={handleSubmit} style={{ display: 'flex', flexDirection: 'column', maxWidth: 400 }}>
        <input name="name" placeholder="Name" onChange={handleChange} required />
        <input name="description" placeholder="Description" onChange={handleChange} required />
        <input name="pricePerUnit" type="number" placeholder="Price per unit" onChange={handleChange} required />
        <input name="minOrderQty" type="number" placeholder="Min Order Quantity" onChange={handleChange} required />
        <input name="imageUrl" placeholder="Image URL" onChange={handleChange} />
        <button type="submit">Add Product</button>
      </form>
    </div>
  );
}

export default App;
