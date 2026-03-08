import { useState, useEffect, useRef } from "react";
import { createClient } from "@supabase/supabase-js";

const SUPABASE_URL = "https://inriodivjpabohrgttab.supabase.co";
const SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImlucmlvZGl2anBhYm9ocmd0dGFiIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzI5NjY0MzQsImV4cCI6MjA4ODU0MjQzNH0.qgyuqtiAw6rWWQEiIt_NJJtMPz3EItF4ADpOR1QBglc";
const TEACHER_EMAIL = "georgiathomas91@gmail.com";

const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

const fileIcon = (type) => {
  if (!type) return "📁";
  if (type.startsWith("image")) return "🖼️";
  if (type.startsWith("audio")) return "🎵";
  if (type.includes("pdf")) return "📄";
  return "📁";
};

const formatSize = (bytes) => {
  if (!bytes) return "";
  if (bytes < 1024) return bytes + " B";
  if (bytes < 1048576) return (bytes / 1024).toFixed(1) + " KB";
  return (bytes / 1048576).toFixed(1) + " MB";
};

const css = `
  @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=DM+Sans:wght@300;400;500;600&display=swap');
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'DM Sans', sans-serif; background: #0d1117; color: #e8e4dc; min-height: 100vh; }

  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes spin { to { transform: rotate(360deg); } }

  .animate { animation: fadeIn 0.3s ease forwards; }
  .spinner { width: 20px; height: 20px; border: 2px solid #1e2d42; border-top-color: #3b7dd8; border-radius: 50%; animation: spin 0.7s linear infinite; display: inline-block; }

  .login-wrap { min-height: 100vh; display: flex; align-items: center; justify-content: center; background: radial-gradient(ellipse at 30% 20%, #1a2744 0%, #0d1117 60%); }
  .login-card { background: #131920; border: 1px solid #1e2d42; border-radius: 20px; padding: 56px 48px; width: 420px; text-align: center; box-shadow: 0 32px 80px rgba(0,0,0,0.5); animation: fadeIn 0.4s ease; }
  .login-badge { display: inline-block; background: #1a3a5c; color: #5ba3d9; font-size: 11px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; padding: 6px 14px; border-radius: 20px; margin-bottom: 28px; }
  .login-title { font-family: 'Playfair Display', serif; font-size: 36px; color: #e8e4dc; margin-bottom: 8px; }
  .login-sub { color: #6b7a8d; font-size: 14px; margin-bottom: 36px; }
  .login-input { width: 100%; padding: 14px 18px; background: #0d1117; border: 1px solid #1e2d42; border-radius: 10px; color: #e8e4dc; font-size: 15px; font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.2s; }
  .login-input:focus { border-color: #3b7dd8; }
  .login-btn { width: 100%; margin-top: 14px; padding: 14px; background: #3b7dd8; border: none; border-radius: 10px; color: white; font-size: 15px; font-weight: 600; font-family: 'DM Sans', sans-serif; cursor: pointer; transition: background 0.2s; display: flex; align-items: center; justify-content: center; gap: 10px; }
  .login-btn:hover:not(:disabled) { background: #4d8ee8; }
  .login-btn:disabled { opacity: 0.6; cursor: not-allowed; }
  .login-error { color: #e06c6c; font-size: 13px; margin-top: 12px; }

  .shell { display: flex; min-height: 100vh; }
  .sidebar { width: 240px; background: #0a0f16; border-right: 1px solid #1a2233; padding: 32px 0; display: flex; flex-direction: column; flex-shrink: 0; }
  .sidebar-logo { padding: 0 24px 32px; border-bottom: 1px solid #1a2233; margin-bottom: 24px; }
  .sidebar-logo-text { font-family: 'Playfair Display', serif; font-size: 22px; color: #e8e4dc; }
  .sidebar-logo-sub { font-size: 11px; color: #3b5070; letter-spacing: 1px; text-transform: uppercase; }
  .sidebar-section { padding: 0 16px; margin-bottom: 8px; }
  .sidebar-label { font-size: 10px; color: #3b5070; letter-spacing: 1.5px; text-transform: uppercase; padding: 0 8px; margin-bottom: 8px; }
  .sidebar-btn { width: 100%; display: flex; align-items: center; gap: 10px; padding: 10px 12px; border-radius: 8px; border: none; background: transparent; color: #6b7a8d; font-size: 14px; font-family: 'DM Sans', sans-serif; cursor: pointer; transition: all 0.15s; text-align: left; }
  .sidebar-btn:hover { background: #131920; color: #e8e4dc; }
  .sidebar-btn.active { background: #1a2744; color: #5ba3d9; }
  .sidebar-btn .count { margin-left: auto; background: #1a2744; color: #5ba3d9; font-size: 11px; padding: 2px 8px; border-radius: 10px; }
  .sidebar-bottom { margin-top: auto; padding: 16px; border-top: 1px solid #1a2233; }
  .user-chip { display: flex; align-items: center; gap: 10px; }
  .user-avatar { width: 36px; height: 36px; border-radius: 50%; background: #1a3a5c; display: flex; align-items: center; justify-content: center; font-size: 14px; font-weight: 600; color: #5ba3d9; flex-shrink: 0; }
  .user-info { flex: 1; min-width: 0; }
  .user-name { font-size: 13px; font-weight: 500; color: #e8e4dc; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .user-role { font-size: 11px; color: #3b5070; }
  .logout-btn { background: none; border: none; color: #3b5070; cursor: pointer; font-size: 18px; padding: 4px; }
  .logout-btn:hover { color: #e06c6c; }

  .main { flex: 1; overflow: auto; }
  .topbar { padding: 24px 36px; border-bottom: 1px solid #1a2233; display: flex; align-items: center; justify-content: space-between; background: #0d1117; position: sticky; top: 0; z-index: 10; }
  .topbar-title { font-family: 'Playfair Display', serif; font-size: 26px; color: #e8e4dc; }
  .topbar-sub { font-size: 13px; color: #3b5070; margin-top: 2px; }
  .topbar-actions { display: flex; gap: 10px; align-items: center; }

  .btn-primary { padding: 10px 20px; background: #3b7dd8; border: none; border-radius: 8px; color: white; font-size: 14px; font-weight: 500; font-family: 'DM Sans', sans-serif; cursor: pointer; transition: background 0.2s; display: flex; align-items: center; gap: 6px; }
  .btn-primary:hover:not(:disabled) { background: #4d8ee8; }
  .btn-primary:disabled { opacity: 0.5; cursor: not-allowed; }
  .btn-ghost { padding: 8px 16px; background: transparent; border: 1px solid #1e2d42; border-radius: 8px; color: #6b7a8d; font-size: 13px; font-family: 'DM Sans', sans-serif; cursor: pointer; transition: all 0.2s; }
  .btn-ghost:hover { border-color: #3b7dd8; color: #5ba3d9; }
  .btn-danger { padding: 6px 12px; background: transparent; border: 1px solid #3a1a1a; border-radius: 6px; color: #e06c6c; font-size: 12px; font-family: 'DM Sans', sans-serif; cursor: pointer; transition: all 0.2s; }
  .btn-danger:hover { background: #2a0e0e; }

  .content { padding: 32px 36px; }
  .stat-row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; margin-bottom: 36px; }
  .stat-card { background: #131920; border: 1px solid #1a2233; border-radius: 14px; padding: 24px; animation: fadeIn 0.3s ease; }
  .stat-num { font-family: 'Playfair Display', serif; font-size: 40px; color: #5ba3d9; }
  .stat-label { font-size: 13px; color: #6b7a8d; margin-top: 4px; }
  .section-title { font-size: 12px; color: #3b5070; letter-spacing: 1.5px; text-transform: uppercase; margin-bottom: 16px; }

  .student-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 16px; }
  .student-card { background: #131920; border: 1px solid #1a2233; border-radius: 14px; padding: 20px; cursor: pointer; transition: all 0.2s; animation: fadeIn 0.3s ease; }
  .student-card:hover { border-color: #3b7dd8; transform: translateY(-2px); }
  .student-card-head { display: flex; align-items: center; gap: 12px; margin-bottom: 14px; }
  .student-avatar-lg { width: 42px; height: 42px; border-radius: 50%; background: #1a2744; display: flex; align-items: center; justify-content: center; font-size: 16px; font-weight: 700; color: #5ba3d9; flex-shrink: 0; }
  .student-name { font-size: 15px; font-weight: 600; color: #e8e4dc; }
  .student-email { font-size: 12px; color: #3b5070; }
  .file-count-badge { display: inline-block; background: #1a2744; color: #5ba3d9; font-size: 12px; padding: 4px 12px; border-radius: 20px; }
  .card-actions { display: flex; justify-content: space-between; align-items: center; margin-top: 14px; }

  .add-student-card { background: #131920; border: 2px dashed #1e2d42; border-radius: 14px; padding: 24px; }
  .input-row { display: flex; gap: 10px; margin-bottom: 10px; }
  .form-input { flex: 1; padding: 10px 14px; background: #0d1117; border: 1px solid #1e2d42; border-radius: 8px; color: #e8e4dc; font-size: 14px; font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.2s; }
  .form-input:focus { border-color: #3b7dd8; }

  .dropzone { border: 2px dashed #1e2d42; border-radius: 14px; padding: 40px; text-align: center; transition: all 0.2s; cursor: pointer; margin-bottom: 24px; background: #0d1117; }
  .dropzone.over { border-color: #3b7dd8; background: #0d1520; }
  .dropzone-icon { font-size: 40px; margin-bottom: 12px; }
  .dropzone-text { font-size: 15px; color: #6b7a8d; }
  .dropzone-sub { font-size: 12px; color: #3b5070; margin-top: 6px; }

  .file-list { display: flex; flex-direction: column; gap: 12px; }
  .file-row { background: #131920; border: 1px solid #1a2233; border-radius: 12px; padding: 16px 20px; display: flex; align-items: center; gap: 16px; animation: fadeIn 0.3s ease; }
  .file-icon { font-size: 28px; flex-shrink: 0; }
  .file-name { font-size: 14px; font-weight: 500; color: #e8e4dc; }
  .file-meta { font-size: 12px; color: #3b5070; margin-top: 2px; }
  .assign-tags { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 10px; }
  .assign-tag { padding: 4px 10px; border-radius: 20px; font-size: 11px; font-weight: 500; cursor: pointer; border: 1px solid #1e2d42; color: #6b7a8d; background: transparent; transition: all 0.15s; font-family: 'DM Sans', sans-serif; }
  .assign-tag.assigned { background: #1a2744; border-color: #3b7dd8; color: #5ba3d9; }
  .assign-tag:hover { border-color: #3b7dd8; color: #5ba3d9; }

  .toggle-row { display: flex; align-items: center; gap: 12px; padding: 14px 16px; transition: background 0.15s; }
  .toggle-row:hover { background: #0d1117; }
  .toggle { position: relative; width: 40px; height: 22px; flex-shrink: 0; }
  .toggle input { opacity: 0; width: 0; height: 0; }
  .toggle-slider { position: absolute; inset: 0; background: #1e2d42; border-radius: 22px; cursor: pointer; transition: background 0.2s; }
  .toggle-slider:before { content: ''; position: absolute; width: 16px; height: 16px; left: 3px; top: 3px; background: white; border-radius: 50%; transition: transform 0.2s; }
  .toggle input:checked + .toggle-slider { background: #3b7dd8; }
  .toggle input:checked + .toggle-slider:before { transform: translateX(18px); }

  .student-file-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 16px; }
  .student-file-card { background: #131920; border: 1px solid #1a2233; border-radius: 14px; padding: 24px 20px; text-align: center; cursor: pointer; transition: all 0.2s; text-decoration: none; display: block; animation: fadeIn 0.3s ease; }
  .student-file-card:hover { border-color: #3b7dd8; transform: translateY(-2px); }
  .student-file-icon { font-size: 40px; margin-bottom: 12px; }
  .student-file-name { font-size: 13px; color: #e8e4dc; word-break: break-word; }

  .empty-state { text-align: center; padding: 80px 20px; }
  .empty-icon { font-size: 56px; margin-bottom: 16px; opacity: 0.4; }
  .empty-text { font-size: 16px; color: #3b5070; }

  .toast { position: fixed; bottom: 24px; right: 24px; background: #1a2744; border: 1px solid #3b7dd8; color: #5ba3d9; padding: 12px 20px; border-radius: 10px; font-size: 14px; animation: fadeIn 0.3s ease; z-index: 100; }
  .toast.error { background: #2a0e0e; border-color: #e06c6c; color: #e06c6c; }

  .loading-screen { min-height: 100vh; display: flex; align-items: center; justify-content: center; background: #0d1117; flex-direction: column; gap: 16px; }
  .loading-text { color: #3b5070; font-size: 14px; }

  .progress-bar { height: 3px; background: #1e2d42; border-radius: 3px; overflow: hidden; margin: 16px auto 0; max-width: 300px; }
  .progress-fill { height: 100%; background: #3b7dd8; border-radius: 3px; transition: width 0.3s; }
`;

export default function App() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [emailInput, setEmailInput] = useState("");
  const [loginLoading, setLoginLoading] = useState(false);
  const [loginError, setLoginError] = useState("");

  const [students, setStudents] = useState([]);
  const [files, setFiles] = useState([]);
  const [assignments, setAssignments] = useState([]);

  const [view, setView] = useState("dashboard");
  const [selectedStudent, setSelectedStudent] = useState(null);
  const [dragOver, setDragOver] = useState(false);
  const [uploading, setUploading] = useState(false);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [addingStudent, setAddingStudent] = useState(false);
  const [newStudentName, setNewStudentName] = useState("");
  const [newStudentEmail, setNewStudentEmail] = useState("");
  const [toast, setToast] = useState(null);

  const fileRef = useRef();
  const pendingStudentId = useRef(null);

  const showToast = (msg, type = "success") => {
    setToast({ msg, type });
    setTimeout(() => setToast(null), 3000);
  };

  const loadData = async () => {
    const [{ data: s }, { data: f }, { data: a }] = await Promise.all([
      supabase.from("students").select("*").order("created_at"),
      supabase.from("files").select("*").order("created_at"),
      supabase.from("assignments").select("*"),
    ]);
    if (s) setStudents(s);
    if (f) setFiles(f);
    if (a) setAssignments(a);
  };

  useEffect(() => {
    const saved = sessionStorage.getItem("classroom_user");
    if (saved) {
      try { setUser(JSON.parse(saved)); } catch {}
    }
    loadData().finally(() => setLoading(false));
  }, []);

  const handleLogin = async () => {
    const email = emailInput.trim().toLowerCase();
    if (!email) return;
    setLoginLoading(true);
    setLoginError("");

    if (email === TEACHER_EMAIL) {
      const u = { role: "teacher", email };
      setUser(u);
      sessionStorage.setItem("classroom_user", JSON.stringify(u));
      setLoginLoading(false);
      return;
    }

    const { data: student } = await supabase.from("students").select("*").eq("email", email).single();
    if (!student) {
      setLoginError("Email not recognised. Please check with your teacher.");
      setLoginLoading(false);
      return;
    }

    const u = { role: "student", email, id: student.id, name: student.name };
    setUser(u);
    sessionStorage.setItem("classroom_user", JSON.stringify(u));
    setLoginLoading(false);
  };

  const handleLogout = () => {
    setUser(null);
    sessionStorage.removeItem("classroom_user");
    setEmailInput("");
  };

  const handleFileUpload = async (rawFiles) => {
    setUploading(true);
    setUploadProgress(0);
    const total = rawFiles.length;

    for (let i = 0; i < total; i++) {
      const file = rawFiles[i];
      const path = `${Date.now()}_${file.name.replace(/\s+/g, "_")}`;

      const { error: uploadError } = await supabase.storage.from("materials").upload(path, file);
      if (uploadError) { showToast(`Failed to upload ${file.name}`, "error"); continue; }

      const { data: inserted } = await supabase.from("files").insert({
        name: file.name, type: file.type, size: file.size, storage_path: path
      }).select().single();

      if (inserted) {
        setFiles((prev) => [...prev, inserted]);
        if (pendingStudentId.current) {
          await supabase.from("assignments").insert({ file_id: inserted.id, student_id: pendingStudentId.current });
          setAssignments((prev) => [...prev, { file_id: inserted.id, student_id: pendingStudentId.current }]);
        }
      }

      setUploadProgress(Math.round(((i + 1) / total) * 100));
    }

    pendingStudentId.current = null;
    setUploading(false);
    showToast(`${total} file${total > 1 ? "s" : ""} uploaded!`);
  };

  const toggleAssign = async (fileId, studentId) => {
    const exists = assignments.find((a) => a.file_id === fileId && a.student_id === studentId);
    if (exists) {
      await supabase.from("assignments").delete().eq("file_id", fileId).eq("student_id", studentId);
      setAssignments((prev) => prev.filter((a) => !(a.file_id === fileId && a.student_id === studentId)));
    } else {
      await supabase.from("assignments").insert({ file_id: fileId, student_id: studentId });
      setAssignments((prev) => [...prev, { file_id: fileId, student_id: studentId }]);
    }
  };

  const deleteFile = async (file) => {
    await supabase.storage.from("materials").remove([file.storage_path]);
    await supabase.from("files").delete().eq("id", file.id);
    setFiles((prev) => prev.filter((f) => f.id !== file.id));
    setAssignments((prev) => prev.filter((a) => a.file_id !== file.id));
    showToast("File removed");
  };

  const addStudent = async () => {
    if (!newStudentName.trim() || !newStudentEmail.trim()) return;
    const { data, error } = await supabase.from("students").insert({
      name: newStudentName.trim(), email: newStudentEmail.trim().toLowerCase()
    }).select().single();
    if (error) { showToast("Failed to add student", "error"); return; }
    setStudents((prev) => [...prev, data]);
    setNewStudentName("");
    setNewStudentEmail("");
    setAddingStudent(false);
    showToast(`${data.name} added!`);
  };

  const removeStudent = async (id) => {
    await supabase.from("students").delete().eq("id", id);
    setStudents((prev) => prev.filter((s) => s.id !== id));
    setAssignments((prev) => prev.filter((a) => a.student_id !== id));
    if (selectedStudent?.id === id) { setSelectedStudent(null); setView("dashboard"); }
    showToast("Student removed");
  };

  const studentFiles = (studentId) => {
    const ids = assignments.filter((a) => a.student_id === studentId).map((a) => a.file_id);
    return files.filter((f) => ids.includes(f.id));
  };

  const getFileUrl = (path) => {
    const { data } = supabase.storage.from("materials").getPublicUrl(path);
    return data.publicUrl;
  };

  if (loading) return (
    <>
      <style>{css}</style>
      <div className="loading-screen">
        <div className="spinner" style={{ width: 32, height: 32 }}></div>
        <div className="loading-text">Loading classroom…</div>
      </div>
    </>
  );

  if (!user) return (
    <>
      <style>{css}</style>
      <div className="login-wrap">
        <div className="login-card">
          <div className="login-badge">Learning Portal</div>
          <div className="login-title">Welcome Back</div>
          <div className="login-sub">Enter your email to continue</div>
          <input className="login-input" type="email" placeholder="your@email.com"
            value={emailInput} onChange={(e) => setEmailInput(e.target.value)}
            onKeyDown={(e) => e.key === "Enter" && handleLogin()} autoFocus />
          <button className="login-btn" onClick={handleLogin} disabled={loginLoading}>
            {loginLoading ? <><span className="spinner"></span> Checking…</> : "Continue →"}
          </button>
          {loginError && <div className="login-error">{loginError}</div>}
        </div>
      </div>
    </>
  );

  if (user.role === "student") {
    const myFiles = studentFiles(user.id);
    return (
      <>
        <style>{css}</style>
        <div className="shell">
          <div className="sidebar">
            <div className="sidebar-logo">
              <div className="sidebar-logo-text">Classroom</div>
              <div className="sidebar-logo-sub">Student Portal</div>
            </div>
            <div className="sidebar-section">
              <div className="sidebar-label">Navigation</div>
              <button className="sidebar-btn active">📚 My Materials</button>
            </div>
            <div className="sidebar-bottom">
              <div className="user-chip">
                <div className="user-avatar">{user.name[0].toUpperCase()}</div>
                <div className="user-info">
                  <div className="user-name">{user.name}</div>
                  <div className="user-role">Student</div>
                </div>
                <button className="logout-btn" onClick={handleLogout} title="Log out">↩</button>
              </div>
            </div>
          </div>
          <div className="main">
            <div className="topbar">
              <div>
                <div className="topbar-title">My Materials</div>
                <div className="topbar-sub">{myFiles.length} item{myFiles.length !== 1 ? "s" : ""} available</div>
              </div>
            </div>
            <div className="content">
              {myFiles.length === 0 ? (
                <div className="empty-state">
                  <div className="empty-icon">📭</div>
                  <div className="empty-text">Nothing here yet — your teacher will add materials soon.</div>
                </div>
              ) : (
                <div className="student-file-grid">
                  {myFiles.map((f) => (
                    <a key={f.id} href={getFileUrl(f.storage_path)} target="_blank" rel="noreferrer" className="student-file-card">
                      <div className="student-file-icon">{fileIcon(f.type)}</div>
                      <div className="student-file-name">{f.name}</div>
                      <div style={{ fontSize: 11, color: "#3b5070", marginTop: 8 }}>{formatSize(f.size)}</div>
                    </a>
                  ))}
                </div>
              )}
            </div>
          </div>
        </div>
        {toast && <div className={`toast ${toast.type === "error" ? "error" : ""}`}>{toast.msg}</div>}
      </>
    );
  }

  return (
    <>
      <style>{css}</style>
      <div className="shell">
        <div className="sidebar">
          <div className="sidebar-logo">
            <div className="sidebar-logo-text">Classroom</div>
            <div className="sidebar-logo-sub">Teacher Portal</div>
          </div>
          <div className="sidebar-section">
            <div className="sidebar-label">Views</div>
            <button className={`sidebar-btn ${view === "dashboard" ? "active" : ""}`} onClick={() => { setView("dashboard"); setSelectedStudent(null); }}>🏠 Dashboard</button>
            <button className={`sidebar-btn ${view === "library" ? "active" : ""}`} onClick={() => { setView("library"); setSelectedStudent(null); }}>
              📁 File Library <span className="count">{files.length}</span>
            </button>
          </div>
          <div className="sidebar-section" style={{ marginTop: 8 }}>
            <div className="sidebar-label">Students</div>
            {students.map((s) => (
              <button key={s.id} className={`sidebar-btn ${selectedStudent?.id === s.id ? "active" : ""}`}
                onClick={() => { setSelectedStudent(s); setView("student"); }}>
                👤 {s.name} <span className="count">{studentFiles(s.id).length}</span>
              </button>
            ))}
          </div>
          <div className="sidebar-bottom">
            <div className="user-chip">
              <div className="user-avatar">T</div>
              <div className="user-info">
                <div className="user-name">Teacher</div>
                <div className="user-role">Admin</div>
              </div>
              <button className="logout-btn" onClick={handleLogout} title="Log out">↩</button>
            </div>
          </div>
        </div>

        <div className="main">
          {view === "dashboard" && (
            <>
              <div className="topbar">
                <div>
                  <div className="topbar-title">Dashboard</div>
                  <div className="topbar-sub">Overview of your classroom</div>
                </div>
                <button className="btn-primary" onClick={() => setView("library")}>+ Upload Files</button>
              </div>
              <div className="content">
                <div className="stat-row">
                  <div className="stat-card"><div className="stat-num">{students.length}</div><div className="stat-label">Students</div></div>
                  <div className="stat-card"><div className="stat-num">{files.length}</div><div className="stat-label">Files Uploaded</div></div>
                  <div className="stat-card"><div className="stat-num">{assignments.length}</div><div className="stat-label">Assignments Made</div></div>
                </div>
                <div className="section-title">Your Students</div>
                <div className="student-grid">
                  {students.map((s) => (
                    <div key={s.id} className="student-card" onClick={() => { setSelectedStudent(s); setView("student"); }}>
                      <div className="student-card-head">
                        <div className="student-avatar-lg">{s.name[0].toUpperCase()}</div>
                        <div>
                          <div className="student-name">{s.name}</div>
                          <div className="student-email">{s.email}</div>
                        </div>
                      </div>
                      <div className="card-actions">
                        <span className="file-count-badge">{studentFiles(s.id).length} files</span>
                        <button className="btn-ghost" onClick={(e) => { e.stopPropagation(); setSelectedStudent(s); setView("student"); }}>Manage →</button>
                      </div>
                    </div>
                  ))}
                  {addingStudent ? (
                    <div className="add-student-card">
                      <div style={{ fontSize: 13, color: "#6b7a8d", marginBottom: 12 }}>Add new student</div>
                      <div className="input-row"><input className="form-input" placeholder="Full name" value={newStudentName} onChange={(e) => setNewStudentName(e.target.value)} /></div>
                      <div className="input-row"><input className="form-input" placeholder="Email address" value={newStudentEmail} onChange={(e) => setNewStudentEmail(e.target.value)} onKeyDown={(e) => e.key === "Enter" && addStudent()} /></div>
                      <div style={{ display: "flex", gap: 8 }}>
                        <button className="btn-primary" style={{ flex: 1 }} onClick={addStudent}>Add Student</button>
                        <button className="btn-ghost" onClick={() => setAddingStudent(false)}>Cancel</button>
                      </div>
                    </div>
                  ) : (
                    <div className="student-card" style={{ border: "2px dashed #1e2d42", display: "flex", alignItems: "center", justifyContent: "center", minHeight: 120 }} onClick={() => setAddingStudent(true)}>
                      <div style={{ textAlign: "center", color: "#3b5070" }}>
                        <div style={{ fontSize: 28, marginBottom: 8 }}>+</div>
                        <div style={{ fontSize: 13 }}>Add Student</div>
                      </div>
                    </div>
                  )}
                </div>
              </div>
            </>
          )}

          {view === "library" && (
            <>
              <div className="topbar">
                <div>
                  <div className="topbar-title">File Library</div>
                  <div className="topbar-sub">Upload and assign materials to students</div>
                </div>
              </div>
              <div className="content">
                <div className={`dropzone ${dragOver ? "over" : ""}`}
                  onDragOver={(e) => { e.preventDefault(); setDragOver(true); }}
                  onDragLeave={() => setDragOver(false)}
                  onDrop={(e) => { e.preventDefault(); setDragOver(false); handleFileUpload(e.dataTransfer.files); }}
                  onClick={() => fileRef.current.click()}>
                  <div className="dropzone-icon">{uploading ? "⏳" : "☁️"}</div>
                  <div className="dropzone-text">{uploading ? `Uploading… ${uploadProgress}%` : "Drop files here or click to upload"}</div>
                  <div className="dropzone-sub">PDFs, Images, Audio files supported</div>
                  {uploading && <div className="progress-bar"><div className="progress-fill" style={{ width: uploadProgress + "%" }}></div></div>}
                  <input ref={fileRef} type="file" multiple accept=".pdf,image/*,audio/*" style={{ display: "none" }} onChange={(e) => handleFileUpload(e.target.files)} />
                </div>
                {files.length === 0 ? (
                  <div className="empty-state"><div className="empty-icon">📂</div><div className="empty-text">No files yet — upload something above.</div></div>
                ) : (
                  <div className="file-list">
                    <div className="section-title">All Files — click student tags to assign/unassign</div>
                    {files.map((f) => (
                      <div key={f.id} className="file-row">
                        <div className="file-icon">{fileIcon(f.type)}</div>
                        <div style={{ flex: 1, minWidth: 0 }}>
                          <div className="file-name">{f.name}</div>
                          <div className="file-meta">{formatSize(f.size)}</div>
                          <div className="assign-tags">
                            {students.map((s) => {
                              const assigned = assignments.some((a) => a.file_id === f.id && a.student_id === s.id);
                              return (
                                <button key={s.id} className={`assign-tag ${assigned ? "assigned" : ""}`} onClick={() => toggleAssign(f.id, s.id)}>
                                  {assigned ? "✓ " : ""}{s.name}
                                </button>
                              );
                            })}
                          </div>
                        </div>
                        <button className="btn-danger" onClick={() => deleteFile(f)}>Remove</button>
                      </div>
                    ))}
                  </div>
                )}
              </div>
            </>
          )}

          {view === "student" && selectedStudent && (
            <>
              <div className="topbar">
                <div>
                  <div className="topbar-title">{selectedStudent.name}</div>
                  <div className="topbar-sub">{selectedStudent.email} · {studentFiles(selectedStudent.id).length} files assigned</div>
                </div>
                <div className="topbar-actions">
                  <button className="btn-ghost" onClick={() => { pendingStudentId.current = selectedStudent.id; fileRef.current.click(); }}>+ Upload for {selectedStudent.name}</button>
                  <button className="btn-danger" onClick={() => removeStudent(selectedStudent.id)}>Remove Student</button>
                </div>
                <input ref={fileRef} type="file" multiple accept=".pdf,image/*,audio/*" style={{ display: "none" }} onChange={(e) => handleFileUpload(e.target.files)} />
              </div>
              <div className="content">
                <div className="section-title" style={{ marginBottom: 20 }}>Toggle files for {selectedStudent.name}</div>
                {files.length === 0 ? (
                  <div className="empty-state"><div className="empty-icon">📁</div><div className="empty-text">No files in library yet.</div></div>
                ) : (
                  <div style={{ background: "#131920", border: "1px solid #1a2233", borderRadius: 14, overflow: "hidden" }}>
                    {files.map((f, i) => {
                      const assigned = assignments.some((a) => a.file_id === f.id && a.student_id === selectedStudent.id);
                      return (
                        <div key={f.id} className="toggle-row" style={{ borderTop: i > 0 ? "1px solid #1a2233" : "none" }}>
                          <span style={{ fontSize: 22 }}>{fileIcon(f.type)}</span>
                          <div style={{ flex: 1 }}>
                            <div style={{ fontSize: 14, color: "#e8e4dc" }}>{f.name}</div>
                            <div style={{ fontSize: 12, color: "#3b5070" }}>{formatSize(f.size)}</div>
                          </div>
                          <label className="toggle">
                            <input type="checkbox" checked={assigned} onChange={() => toggleAssign(f.id, selectedStudent.id)} />
                            <span className="toggle-slider"></span>
                          </label>
                        </div>
                      );
                    })}
                  </div>
                )}
              </div>
            </>
          )}
        </div>
      </div>
      {toast && <div className={`toast ${toast.type === "error" ? "error" : ""}`}>{toast.msg}</div>}
    </>
  );
}
