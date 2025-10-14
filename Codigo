# REACT---Sistema-de-reembolso

import { useState } from "react";
import { BrowserRouter as Router, Routes, Route, Link, useParams } from "react-router-dom";

export default function App() {
  const [reimbursements, setReimbursements] = useState([]);
  const [search, setSearch] = useState("");
  const [page, setPage] = useState(1);
  const itemsPerPage = 5;

  // --- Formulário de Reembolso ---
  function ReimbursementForm() {
    const [name, setName] = useState("");
    const [value, setValue] = useState("");
    const [category, setCategory] = useState("");
    const [date, setDate] = useState("");
    const [receipt, setReceipt] = useState(null);

    function handleSubmit(e) {
      e.preventDefault();
      if (!name || !value || !category || !date) {
        alert("Preencha todos os campos!");
        return;
      }
      const newReimbursement = {
        id: Date.now(),
        name,
        value,
        category,
        date,
        receipt,
      };
      setReimbursements((prev) => [...prev, newReimbursement]);
      setName("");
      setValue("");
      setCategory("");
      setDate("");
      setReceipt(null);
    }

    return (
      <form onSubmit={handleSubmit} style={styles.form}>
        <input
          type="text"
          placeholder="Nome do solicitante"
          value={name}
          onChange={(e) => setName(e.target.value)}
          style={styles.input}
        />
        <input
          type="number"
          placeholder="Valor"
          value={value}
          onChange={(e) => setValue(e.target.value)}
          style={styles.input}
        />
        <input
          type="text"
          placeholder="Categoria"
          value={category}
          onChange={(e) => setCategory(e.target.value)}
          style={styles.input}
        />
        <input
          type="date"
          value={date}
          onChange={(e) => setDate(e.target.value)}
          style={styles.input}
        />
        <input
          type="file"
          onChange={(e) => setReceipt(e.target.files[0])}
          style={styles.input}
        />
        <button type="submit" style={styles.button}>Adicionar Reembolso</button>
      </form>
    );
  }

  // --- Lista e Paginação ---
  const filtered = reimbursements.filter((r) =>
    r.name.toLowerCase().includes(search.toLowerCase())
  );
  const totalPages = Math.ceil(filtered.length / itemsPerPage);
  const paginated = filtered.slice((page - 1) * itemsPerPage, page * itemsPerPage);

  function handleRemove(id) {
    setReimbursements((prev) => prev.filter((r) => r.id !== id));
  }

  function Pagination() {
    return (
      <div style={{ marginTop: 10, textAlign: "center" }}>
        {Array.from({ length: totalPages }, (_, i) => (
          <button
            key={i}
            onClick={() => setPage(i + 1)}
            style={{
              margin: 2,
              padding: "5px 10px",
              backgroundColor: i + 1 === page ? "#e91e63" : "#ccc",
              color: "#fff",
              border: "none",
              borderRadius: 4,
              cursor: "pointer",
            }}
          >
            {i + 1}
          </button>
        ))}
      </div>
    );
  }

  function ReimbursementList() {
    return (
      <div>
        <input
          type="text"
          placeholder="Buscar por nome"
          value={search}
          onChange={(e) => setSearch(e.target.value)}
          style={{ ...styles.input, marginBottom: 10 }}
        />
        {paginated.length === 0 ? (
          <p style={{ textAlign: "center" }}>Nenhum reembolso encontrado.</p>
        ) : (
          <ul style={styles.list}>
            {paginated.map((r) => (
              <li key={r.id} style={styles.item}>
                <div>
                  <Link to={`/detail/${r.id}`} style={{ fontWeight: "bold", color: "#e91e63" }}>
                    {r.name}
                  </Link>{" "}
                  — {r.category} — R${r.value}
                  <br />
                  <span>{r.date}</span>
                </div>
                <button onClick={() => handleRemove(r.id)} style={styles.removeButton}>✖</button>
              </li>
            ))}
          </ul>
        )}
        <Pagination />
      </div>
    );
  }

  // --- Página de Detalhes ---
  function ReimbursementDetail() {
    const { id } = useParams();
    const r = reimbursements.find((item) => item.id === parseInt(id));
    if (!r) return <p>Reembolso não encontrado.</p>;

    return (
      <div style={{ padding: 20 }}>
        <h2>{r.name}</h2>
        <p><strong>Valor:</strong> R${r.value}</p>
        <p><strong>Categoria:</strong> {r.category}</p>
        <p><strong>Data:</strong> {r.date}</p>
        {r.receipt && (
          <p>
            <strong>Recibo:</strong>{" "}
            <a href={URL.createObjectURL(r.receipt)} target="_blank" rel="noreferrer">
              {r.receipt.name}
            </a>
          </p>
        )}
        <Link to="/" style={{ color: "#e91e63" }}>Voltar</Link>
      </div>
    );
  }

  // --- JSX principal ---
  return (
    <Router>
      <div style={styles.container}>
        <h1 style={styles.title}>💼 Sistema de Reembolsos</h1>
        <Routes>
          <Route path="/" element={
            <>
              <ReimbursementForm />
              <ReimbursementList />
            </>
          } />
          <Route path="/detail/:id" element={<ReimbursementDetail />} />
        </Routes>
      </div>
    </Router>
  );
}

// --- Estilos ---
const styles = {
  container: {
    maxWidth: "600px",
    margin: "40px auto",
    padding: "20px",
    background: "#fff",
    borderRadius: "12px",
    boxShadow: "0 4px 10px rgba(0,0,0,0.1)",
    fontFamily: "sans-serif",
  },
  title: {
    textAlign: "center",
    color: "#e91e63",
  },
  form: {
    display: "flex",
    flexDirection: "column",
    gap: "10px",
    marginBottom: "20px",
  },
  input: {
    padding: "10px",
    border: "1px solid #ccc",
    borderRadius: "6px",
  },
  button: {
    padding: "10px",
    backgroundColor: "#e91e63",
    color: "#fff",
    border: "none",
    borderRadius: "6px",
    cursor: "pointer",
  },
  list: {
    listStyle: "none",
    padding: 0,
    display: "flex",
    flexDirection: "column",
    gap: "10px",
  },
  item: {
    display: "flex",
    justifyContent: "space-between",
    alignItems: "center",
    padding: "10px 14px",
    borderRadius: "8px",
    background: "#f8f8f8",
    border: "1px solid #eee",
  },
  removeButton: {
    background: "transparent",
    border: "none",
    fontSize: "16px",
    color: "#e91e63",
    cursor: "pointer",
  },
};
