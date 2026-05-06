<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern CRM - AI-Powered Contact Management</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* CSS Variables for Light/Dark Mode */
        :root {
            /* Light mode colors */
            --color-background-primary: #ffffff;
            --color-background-secondary: #f5f5f5;
            --color-background-tertiary: #f9f9f9;
            --color-background-info: #e3f2fd;
            --color-background-success: #e8f5e9;
            --color-background-danger: #ffebee;
            --color-background-warning: #fff3e0;

            --color-text-primary: #1a1a1a;
            --color-text-secondary: #666666;
            --color-text-tertiary: #999999;
            --color-text-info: #1976d2;
            --color-text-success: #388e3c;
            --color-text-danger: #d32f2f;
            --color-text-warning: #f57c00;

            --color-border-tertiary: rgba(0, 0, 0, 0.08);
            --color-border-secondary: rgba(0, 0, 0, 0.15);
            --color-border-primary: rgba(0, 0, 0, 0.25);

            --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
            --font-serif: Georgia, 'Times New Roman', serif;
            --font-mono: 'Monaco', 'Menlo', 'Ubuntu Mono', 'Consolas', source-code-pro, monospace;

            --border-radius-md: 8px;
            --border-radius-lg: 12px;
            --border-radius-xl: 16px;
        }

        @media (prefers-color-scheme: dark) {
            :root {
                /* Dark mode colors */
                --color-background-primary: #1a1a1a;
                --color-background-secondary: #2d2d2d;
                --color-background-tertiary: #0f0f0f;
                --color-background-info: #1e3a5f;
                --color-background-success: #1b4620;
                --color-background-danger: #5f1f1f;
                --color-background-warning: #5f3a1f;

                --color-text-primary: #e4e4e4;
                --color-text-secondary: #b0b0b0;
                --color-text-tertiary: #808080;
                --color-text-info: #64b5f6;
                --color-text-success: #81c784;
                --color-text-danger: #ef5350;
                --color-text-warning: #ffb74d;

                --color-border-tertiary: rgba(255, 255, 255, 0.08);
                --color-border-secondary: rgba(255, 255, 255, 0.15);
                --color-border-primary: rgba(255, 255, 255, 0.25);
            }
        }

        html, body {
            width: 100%;
            height: 100%;
            font-family: var(--font-sans);
            background: var(--color-background-tertiary);
            color: var(--color-text-primary);
        }

        #root {
            width: 100%;
            height: 100%;
        }

        /* Scrollbar Styling */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        ::-webkit-scrollbar-track {
            background: transparent;
        }

        ::-webkit-scrollbar-thumb {
            background: var(--color-border-secondary);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--color-border-primary);
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <!-- React and Dependencies -->
    <script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/react.production.min.js"></script>
    <script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/react-dom.production.min.js"></script>
    
    <!-- Babel for JSX Transformation -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.5/babel.min.js"></script>

    <!-- Lucide React Icons (CDN) -->
    <script src="https://cdn.jsdelivr.net/npm/lucide@latest"></script>

    <script type="text/babel">
        // Lucide Icons - Define the icons we need
        const Trash2 = () => <i className="fas fa-trash-alt"></i>;
        const Plus = () => <i className="fas fa-plus"></i>;
        const Download = () => <i className="fas fa-download"></i>;
        const Upload = () => <i className="fas fa-upload"></i>;
        const Mail = () => <i className="fas fa-envelope"></i>;
        const Settings = () => <i className="fas fa-cog"></i>;
        const LogOut = () => <i className="fas fa-sign-out-alt"></i>;
        const Menu = () => <i className="fas fa-bars"></i>;
        const X = () => <i className="fas fa-times"></i>;
        const Send = () => <i className="fas fa-paper-plane"></i>;
        const Copy = () => <i className="fas fa-copy"></i>;
        const RefreshCw = () => <i className="fas fa-sync-alt"></i>;
        const Filter = () => <i className="fas fa-filter"></i>;
        const Archive = () => <i className="fas fa-archive"></i>;

        const ModernCRM = () => {
            // ==================== STATE MANAGEMENT ====================
            const [activeTab, setActiveTab] = React.useState('contacts');
            const [sidebarOpen, setSidebarOpen] = React.useState(true);
            
            // Companies & Contacts
            const [companies, setCompanies] = React.useState([
                {
                    id: '1',
                    name: 'Acme Corp',
                    contacts: [
                        { id: 'c1', name: 'John Smith', phone: '555-0101', notes: 'Decision maker', isLead: false },
                        { id: 'c2', name: 'Sarah Johnson', phone: '555-0102', notes: 'Finance contact', isLead: false }
                    ]
                },
                {
                    id: '2',
                    name: 'TechStart Inc',
                    contacts: [
                        { id: 'c3', name: 'Mike Chen', phone: '555-0103', notes: 'CEO', isLead: false }
                    ]
                }
            ]);

            // Leads (converted contacts)
            const [leads, setLeads] = React.useState([
                {
                    id: 'lead1',
                    contactName: 'John Smith',
                    originCompany: 'Acme Corp',
                    insuranceScope: 'Life / Health',
                    broker: 'Yes - ABC Brokers',
                    renewalDate: '2025-06-15',
                    expectedBusiness: 'Health Insurance',
                    nextSteps: 'Active List',
                    status: 'active',
                    createdAt: new Date().toISOString()
                }
            ]);

            // Email Assistant
            const [emailDraft, setEmailDraft] = React.useState('');
            const [emailTone, setEmailTone] = React.useState('professional');
            const [emailHistory, setEmailHistory] = React.useState([]);
            const [showEmailAssistant, setShowEmailAssistant] = React.useState(false);

            // Files
            const [uploadedFiles, setUploadedFiles] = React.useState([
                { id: 'f1', name: 'Q4_Budget.xlsx', type: 'Excel', company: 'Acme Corp', size: '2.4 MB', uploadDate: '2025-01-15' }
            ]);
            const fileInputRef = React.useRef(null);

            // UI State
            const [selectedCompany, setSelectedCompany] = React.useState(null);
            const [showLeadForm, setShowLeadForm] = React.useState(false);
            const [selectedContact, setSelectedContact] = React.useState(null);

            // ==================== LEAD CONVERSION FORM ====================
            const [leadForm, setLeadForm] = React.useState({
                contactName: '',
                insuranceScope: 'Life Insurance',
                broker: 'No',
                brokerName: '',
                renewalDate: '',
                expectedBusiness: 'Health Insurance',
                nextSteps: 'Active List'
            });

            const handleConvertToLead = (contact, company) => {
                setSelectedContact(contact);
                setLeadForm({
                    ...leadForm,
                    contactName: contact.name
                });
                setShowLeadForm(true);
            };

            const submitLeadForm = () => {
                const newLead = {
                    id: `lead${Date.now()}`,
                    contactName: leadForm.contactName,
                    originCompany: selectedContact ? companies.find(c => c.contacts.find(con => con.id === selectedContact.id))?.name : 'Unknown',
                    insuranceScope: leadForm.insuranceScope,
                    broker: leadForm.broker === 'Yes' ? `Yes - ${leadForm.brokerName}` : 'No',
                    renewalDate: leadForm.renewalDate,
                    expectedBusiness: leadForm.expectedBusiness,
                    nextSteps: leadForm.nextSteps,
                    status: 'active',
                    createdAt: new Date().toISOString()
                };

                setLeads([...leads, newLead]);
                setShowLeadForm(false);
                
                // Mark contact as lead
                const updatedCompanies = companies.map(c => ({
                    ...c,
                    contacts: c.contacts.map(con => 
                        con.id === selectedContact.id ? { ...con, isLead: true } : con
                    )
                }));
                setCompanies(updatedCompanies);
                setSelectedContact(null);
            };

            // ==================== COMPANY & CONTACT MANAGEMENT ====================
            const addCompany = () => {
                const newCompany = {
                    id: `company${Date.now()}`,
                    name: 'New Company',
                    contacts: []
                };
                setCompanies([...companies, newCompany]);
            };

            const addContact = (companyId) => {
                const newContact = {
                    id: `contact${Date.now()}`,
                    name: 'New Contact',
                    phone: '',
                    notes: '',
                    isLead: false
                };
                const updatedCompanies = companies.map(c =>
                    c.id === companyId ? { ...c, contacts: [...c.contacts, newContact] } : c
                );
                setCompanies(updatedCompanies);
            };

            const updateCompanyName = (companyId, newName) => {
                const updatedCompanies = companies.map(c =>
                    c.id === companyId ? { ...c, name: newName } : c
                );
                setCompanies(updatedCompanies);
            };

            const updateContact = (companyId, contactId, updates) => {
                const updatedCompanies = companies.map(c =>
                    c.id === companyId
                        ? {
                            ...c,
                            contacts: c.contacts.map(con =>
                                con.id === contactId ? { ...con, ...updates } : con
                            )
                        }
                        : c
                );
                setCompanies(updatedCompanies);
            };

            const deleteContact = (companyId, contactId) => {
                const updatedCompanies = companies.map(c =>
                    c.id === companyId
                        ? { ...c, contacts: c.contacts.filter(con => con.id !== contactId) }
                        : c
                );
                setCompanies(updatedCompanies);
            };

            const deleteCompany = (companyId) => {
                setCompanies(companies.filter(c => c.id !== companyId));
            };

            // ==================== EMAIL ASSISTANT ====================
            const improveEmail = async (instruction) => {
                const toneDescriptions = {
                    professional: 'formal and professional tone',
                    friendly: 'warm and friendly tone',
                    assertive: 'confident and assertive tone',
                    casual: 'casual and conversational tone'
                };

                if (!emailDraft.trim()) {
                    alert('Please write something first!');
                    return;
                }

                try {
                    const prompt = instruction || `Improve this email with a ${toneDescriptions[emailTone]} tone:\n\n"${emailDraft}"`;
                    
                    const response = await fetch('https://api.anthropic.com/v1/messages', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            model: 'claude-sonnet-4-20250514',
                            max_tokens: 1000,
                            messages: [{ role: 'user', content: prompt }]
                        })
                    });

                    const data = await response.json();
                    const improvedText = data.content[0].text;
                    
                    setEmailHistory([...emailHistory, { original: emailDraft, improved: improvedText, tone: emailTone }]);
                    setEmailDraft(improvedText);
                } catch (error) {
                    console.error('Error:', error);
                    alert('Error calling Claude API. Make sure you have CORS enabled or use the Anthropic SDK.');
                }
            };

            // ==================== FILE MANAGEMENT ====================
            const handleFileUpload = (e) => {
                const files = Array.from(e.target.files);
                files.forEach(file => {
                    const newFile = {
                        id: `file${Date.now()}`,
                        name: file.name,
                        type: file.type.split('/')[1]?.toUpperCase() || 'File',
                        company: selectedCompany ? companies.find(c => c.id === selectedCompany)?.name : 'Uncategorized',
                        size: (file.size / 1024 / 1024).toFixed(1) + ' MB',
                        uploadDate: new Date().toISOString().split('T')[0]
                    };
                    setUploadedFiles([...uploadedFiles, newFile]);
                });
            };

            const deleteFile = (fileId) => {
                setUploadedFiles(uploadedFiles.filter(f => f.id !== fileId));
            };

            // ==================== UI COMPONENTS ====================
            const CompanyCard = ({ company, index }) => (
                <div
                    key={company.id}
                    style={{
                        background: 'var(--color-background-primary)',
                        border: '0.5px solid var(--color-border-tertiary)',
                        borderRadius: 'var(--border-radius-lg)',
                        padding: '1.25rem',
                        minHeight: '280px',
                        display: 'flex',
                        flexDirection: 'column',
                        gap: '12px',
                        transition: 'all 0.2s ease',
                        cursor: 'pointer'
                    }}
                    onMouseEnter={(e) => {
                        e.currentTarget.style.borderColor = 'var(--color-border-secondary)';
                        e.currentTarget.style.boxShadow = '0 2px 8px rgba(0,0,0,0.08)';
                    }}
                    onMouseLeave={(e) => {
                        e.currentTarget.style.borderColor = 'var(--color-border-tertiary)';
                        e.currentTarget.style.boxShadow = 'none';
                    }}
                >
                    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'start' }}>
                        <input
                            type="text"
                            value={company.name}
                            onChange={(e) => updateCompanyName(company.id, e.target.value)}
                            style={{
                                fontSize: '16px',
                                fontWeight: '500',
                                color: 'var(--color-text-primary)',
                                border: 'none',
                                background: 'transparent',
                                padding: '0',
                                flex: 1,
                                outline: 'none',
                                cursor: 'text'
                            }}
                        />
                        <button
                            onClick={() => deleteCompany(company.id)}
                            style={{
                                background: 'transparent',
                                border: 'none',
                                cursor: 'pointer',
                                color: 'var(--color-text-secondary)',
                                fontSize: '18px',
                                padding: '0',
                                display: 'flex',
                                alignItems: 'center'
                            }}
                        >
                            <Trash2 />
                        </button>
                    </div>

                    <div style={{ display: 'flex', flexDirection: 'column', gap: '8px', flex: 1 }}>
                        {company.contacts.length === 0 ? (
                            <p style={{ fontSize: '13px', color: 'var(--color-text-tertiary)', margin: '0' }}>
                                No contacts yet
                            </p>
                        ) : (
                            company.contacts.map(contact => (
                                <div
                                    key={contact.id}
                                    style={{
                                        background: contact.isLead ? 'rgba(34, 197, 94, 0.08)' : 'var(--color-background-secondary)',
                                        border: contact.isLead ? '1px solid rgba(34, 197, 94, 0.3)' : '0.5px solid var(--color-border-tertiary)',
                                        borderRadius: 'var(--border-radius-md)',
                                        padding: '8px 10px',
                                        fontSize: '13px'
                                    }}
                                >
                                    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'start', gap: '8px' }}>
                                        <div style={{ flex: 1 }}>
                                            <input
                                                type="text"
                                                value={contact.name}
                                                onChange={(e) => updateContact(company.id, contact.id, { name: e.target.value })}
                                                style={{
                                                    fontSize: '13px',
                                                    fontWeight: '500',
                                                    color: 'var(--color-text-primary)',
                                                    border: 'none',
                                                    background: 'transparent',
                                                    padding: '0',
                                                    width: '100%',
                                                    outline: 'none',
                                                    cursor: 'text',
                                                    marginBottom: '4px'
                                                }}
                                            />
                                            <input
                                                type="text"
                                                value={contact.phone}
                                                onChange={(e) => updateContact(company.id, contact.id, { phone: e.target.value })}
                                                style={{
                                                    fontSize: '12px',
                                                    color: 'var(--color-text-secondary)',
                                                    border: 'none',
                                                    background: 'transparent',
                                                    padding: '0',
                                                    width: '100%',
                                                    outline: 'none',
                                                    cursor: 'text'
                                                }}
                                                placeholder="Phone"
                                            />
                                            {contact.notes && (
                                                <p style={{ fontSize: '11px', color: 'var(--color-text-tertiary)', margin: '2px 0 0', fontStyle: 'italic' }}>
                                                    {contact.notes}
                                                </p>
                                            )}
                                        </div>
                                        <div style={{ display: 'flex', gap: '4px' }}>
                                            {!contact.isLead && (
                                                <button
                                                    onClick={() => handleConvertToLead(contact, company)}
                                                    style={{
                                                        background: 'transparent',
                                                        border: '1px solid var(--color-border-info)',
                                                        color: 'var(--color-text-info)',
                                                        borderRadius: 'var(--border-radius-md)',
                                                        padding: '3px 8px',
                                                        fontSize: '10px',
                                                        cursor: 'pointer',
                                                        fontWeight: '500'
                                                    }}
                                                >
                                                    Convert
                                                </button>
                                            )}
                                            <button
                                                onClick={() => deleteContact(company.id, contact.id)}
                                                style={{
                                                    background: 'transparent',
                                                    border: 'none',
                                                    cursor: 'pointer',
                                                    color: 'var(--color-text-secondary)',
                                                    fontSize: '14px',
                                                    padding: '0'
                                                }}
                                            >
                                                <Trash2 />
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            ))
                        )}
                    </div>

                    <button
                        onClick={() => addContact(company.id)}
                        style={{
                            background: 'var(--color-background-secondary)',
                            border: '0.5px solid var(--color-border-secondary)',
                            color: 'var(--color-text-primary)',
                            borderRadius: 'var(--border-radius-md)',
                            padding: '8px',
                            cursor: 'pointer',
                            fontSize: '13px',
                            fontWeight: '500',
                            display: 'flex',
                            alignItems: 'center',
                            justifyContent: 'center',
                            gap: '6px',
                            transition: 'all 0.2s'
                        }}
                        onMouseEnter={(e) => {
                            e.currentTarget.style.background = 'var(--color-background-secondary)';
                            e.currentTarget.style.borderColor = 'var(--color-border-secondary)';
                        }}
                    >
                        <Plus /> Add Contact
                    </button>
                </div>
            );

            // ==================== RENDER ====================
            return (
                <div style={{ display: 'flex', height: '100vh', background: 'var(--color-background-tertiary)', fontFamily: 'var(--font-sans)' }}>
                    {/* Sidebar */}
                    <div
                        style={{
                            width: sidebarOpen ? '240px' : '0',
                            background: 'var(--color-background-secondary)',
                            borderRight: '0.5px solid var(--color-border-tertiary)',
                            overflow: 'hidden',
                            transition: 'width 0.3s ease',
                            display: 'flex',
                            flexDirection: 'column'
                        }}
                    >
                        <div style={{ padding: '1.5rem 1rem', borderBottom: '0.5px solid var(--color-border-tertiary)' }}>
                            <h1 style={{ fontSize: '18px', fontWeight: '600', margin: '0', color: 'var(--color-text-primary)' }}>
                                CRM Hub
                            </h1>
                        </div>

                        <nav style={{ flex: 1, overflow: 'auto', padding: '1rem' }}>
                            {[
                                { id: 'contacts', label: 'Contacts Board' },
                                { id: 'leads', label: 'Leads' },
                                { id: 'email', label: 'Email Assistant' },
                                { id: 'files', label: 'Files' }
                            ].map(item => (
                                <button
                                    key={item.id}
                                    onClick={() => setActiveTab(item.id)}
                                    style={{
                                        width: '100%',
                                        padding: '12px 12px',
                                        background: activeTab === item.id ? 'var(--color-background-info)' : 'transparent',
                                        color: activeTab === item.id ? 'var(--color-text-info)' : 'var(--color-text-primary)',
                                        border: 'none',
                                        borderRadius: 'var(--border-radius-md)',
                                        cursor: 'pointer',
                                        fontSize: '14px',
                                        fontWeight: activeTab === item.id ? '500' : '400',
                                        marginBottom: '8px',
                                        textAlign: 'left',
                                        transition: 'all 0.2s'
                                    }}
                                >
                                    {item.label}
                                </button>
                            ))}
                        </nav>

                        <div style={{ borderTop: '0.5px solid var(--color-border-tertiary)', padding: '1rem' }}>
                            <button
                                style={{
                                    width: '100%',
                                    padding: '10px',
                                    background: 'var(--color-background-secondary)',
                                    border: '0.5px solid var(--color-border-secondary)',
                                    borderRadius: 'var(--border-radius-md)',
                                    cursor: 'pointer',
                                    fontSize: '13px',
                                    color: 'var(--color-text-secondary)'
                                }}
                            >
                                ⚙ Settings
                            </button>
                        </div>
                    </div>

                    {/* Main Content */}
                    <div style={{ flex: 1, display: 'flex', flexDirection: 'column', overflow: 'hidden' }}>
                        {/* Top Bar */}
                        <div style={{
                            background: 'var(--color-background-primary)',
                            borderBottom: '0.5px solid var(--color-border-tertiary)',
                            padding: '1rem 1.5rem',
                            display: 'flex',
                            alignItems: 'center',
                            justifyContent: 'space-between'
                        }}>
                            <button
                                onClick={() => setSidebarOpen(!sidebarOpen)}
                                style={{
                                    background: 'transparent',
                                    border: 'none',
                                    cursor: 'pointer',
                                    fontSize: '20px',
                                    color: 'var(--color-text-primary)'
                                }}
                            >
                                {sidebarOpen ? <X /> : <Menu />}
                            </button>
                            <h2 style={{ fontSize: '18px', fontWeight: '600', margin: '0', color: 'var(--color-text-primary)' }}>
                                {activeTab === 'contacts' && 'Contacts Board'}
                                {activeTab === 'leads' && 'Leads Pipeline'}
                                {activeTab === 'email' && 'AI Email Assistant'}
                                {activeTab === 'files' && 'File Management'}
                            </h2>
                            <div style={{ display: 'flex', gap: '12px' }}>
                                <button style={{ background: 'transparent', border: 'none', cursor: 'pointer', fontSize: '20px', color: 'var(--color-text-secondary)' }}>
                                    <Settings />
                                </button>
                            </div>
                        </div>

                        {/* Content Area */}
                        <div style={{ flex: 1, overflow: 'auto', padding: '2rem' }}>
                            {/* CONTACTS BOARD */}
                            {activeTab === 'contacts' && (
                                <div>
                                    <div style={{ marginBottom: '1.5rem' }}>
                                        <button
                                            onClick={addCompany}
                                            style={{
                                                background: 'var(--color-background-info)',
                                                color: 'var(--color-text-info)',
                                                border: 'none',
                                                borderRadius: 'var(--border-radius-md)',
                                                padding: '10px 16px',
                                                cursor: 'pointer',
                                                fontSize: '14px',
                                                fontWeight: '500',
                                                display: 'flex',
                                                alignItems: 'center',
                                                gap: '8px'
                                            }}
                                        >
                                            <Plus /> Add Company
                                        </button>
                                    </div>

                                    <div style={{
                                        display: 'grid',
                                        gridTemplateColumns: 'repeat(auto-fill, minmax(300px, 1fr))',
                                        gap: '1.5rem'
                                    }}>
                                        {companies.map((company, idx) => (
                                            <CompanyCard key={company.id} company={company} index={idx} />
                                        ))}
                                    </div>
                                </div>
                            )}

                            {/* LEADS */}
                            {activeTab === 'leads' && (
                                <div>
                                    <div style={{
                                        display: 'grid',
                                        gridTemplateColumns: 'repeat(auto-fill, minmax(350px, 1fr))',
                                        gap: '1.5rem'
                                    }}>
                                        {leads.map(lead => (
                                            <div
                                                key={lead.id}
                                                style={{
                                                    background: 'var(--color-background-primary)',
                                                    border: '2px solid rgba(34, 197, 94, 0.3)',
                                                    borderRadius: 'var(--border-radius-lg)',
                                                    padding: '1.5rem',
                                                    boxShadow: '0 2px 8px rgba(34, 197, 94, 0.08)'
                                                }}
                                            >
                                                <div style={{ display: 'flex', alignItems: 'center', gap: '8px', marginBottom: '12px' }}>
                                                    <div style={{
                                                        background: 'rgba(34, 197, 94, 0.1)',
                                                        color: 'rgb(34, 197, 94)',
                                                        borderRadius: '50%',
                                                        width: '28px',
                                                        height: '28px',
                                                        display: 'flex',
                                                        alignItems: 'center',
                                                        justifyContent: 'center',
                                                        fontSize: '12px',
                                                        fontWeight: '600'
                                                    }}>
                                                        ✓
                                                    </div>
                                                    <div>
                                                        <p style={{ fontSize: '14px', fontWeight: '600', margin: '0', color: 'var(--color-text-primary)' }}>
                                                            {lead.contactName}
                                                        </p>
                                                        <p style={{ fontSize: '12px', color: 'var(--color-text-secondary)', margin: '0' }}>
                                                            {lead.originCompany}
                                                        </p>
                                                    </div>
                                                </div>

                                                <div style={{ display: 'flex', flexDirection: 'column', gap: '8px', fontSize: '13px' }}>
                                                    <div>
                                                        <span style={{ color: 'var(--color-text-secondary)' }}>Insurance Scope:</span>
                                                        <span style={{ marginLeft: '8px', color: 'var(--color-text-primary)', fontWeight: '500' }}>
                                                            {lead.insuranceScope}
                                                        </span>
                                                    </div>
                                                    <div>
                                                        <span style={{ color: 'var(--color-text-secondary)' }}>Broker:</span>
                                                        <span style={{ marginLeft: '8px', color: 'var(--color-text-primary)', fontWeight: '500' }}>
                                                            {lead.broker}
                                                        </span>
                                                    </div>
                                                    <div>
                                                        <span style={{ color: 'var(--color-text-secondary)' }}>Renewal:</span>
                                                        <span style={{ marginLeft: '8px', color: 'var(--color-text-primary)', fontWeight: '500' }}>
                                                            {lead.renewalDate}
                                                        </span>
                                                    </div>
                                                    <div>
                                                        <span style={{ color: 'var(--color-text-secondary)' }}>Expected Business:</span>
                                                        <span style={{ marginLeft: '8px', color: 'var(--color-text-primary)', fontWeight: '500' }}>
                                                            {lead.expectedBusiness}
                                                        </span>
                                                    </div>
                                                    <div style={{
                                                        marginTop: '8px',
                                                        padding: '8px 12px',
                                                        background: 'rgba(59, 130, 246, 0.1)',
                                                        borderRadius: 'var(--border-radius-md)',
                                                        color: 'rgb(59, 130, 246)',
                                                        fontSize: '12px',
                                                        fontWeight: '500'
                                                    }}>
                                                        Next Step: {lead.nextSteps}
                                                    </div>
                                                </div>
                                            </div>
                                        ))}
                                    </div>
                                    {leads.length === 0 && (
                                        <div style={{
                                            textAlign: 'center',
                                            padding: '3rem',
                                            color: 'var(--color-text-secondary)'
                                        }}>
                                            <p>No leads yet. Convert contacts to leads to get started.</p>
                                        </div>
                                    )}
                                </div>
                            )}

                            {/* EMAIL ASSISTANT */}
                            {activeTab === 'email' && (
                                <div style={{ maxWidth: '800px' }}>
                                    <div style={{
                                        background: 'var(--color-background-primary)',
                                        border: '0.5px solid var(--color-border-tertiary)',
                                        borderRadius: 'var(--border-radius-lg)',
                                        padding: '1.5rem',
                                        marginBottom: '1.5rem'
                                    }}>
                                        <h3 style={{ fontSize: '16px', fontWeight: '600', marginTop: '0', marginBottom: '12px', color: 'var(--color-text-primary)' }}>
                                            Email Draft
                                        </h3>
                                        <textarea
                                            value={emailDraft}
                                            onChange={(e) => setEmailDraft(e.target.value)}
                                            style={{
                                                width: '100%',
                                                minHeight: '180px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                padding: '12px',
                                                fontSize: '14px',
                                                fontFamily: 'var(--font-sans)',
                                                color: 'var(--color-text-primary)',
                                                background: 'var(--color-background-secondary)',
                                                resize: 'vertical',
                                                outline: 'none'
                                            }}
                                            placeholder="Write your email here..."
                                        />

                                        <div style={{ marginTop: '12px', display: 'flex', gap: '8px', flexWrap: 'wrap' }}>
                                            <label style={{ fontSize: '13px', color: 'var(--color-text-secondary)' }}>
                                                Tone:
                                                <select
                                                    value={emailTone}
                                                    onChange={(e) => setEmailTone(e.target.value)}
                                                    style={{
                                                        marginLeft: '8px',
                                                        padding: '6px 8px',
                                                        borderRadius: 'var(--border-radius-md)',
                                                        border: '0.5px solid var(--color-border-secondary)',
                                                        background: 'var(--color-background-secondary)',
                                                        color: 'var(--color-text-primary)',
                                                        cursor: 'pointer'
                                                    }}
                                                >
                                                    <option value="professional">Professional</option>
                                                    <option value="friendly">Friendly</option>
                                                    <option value="assertive">Assertive</option>
                                                    <option value="casual">Casual</option>
                                                </select>
                                            </label>
                                        </div>

                                        <div style={{ marginTop: '1rem', display: 'flex', gap: '8px', flexWrap: 'wrap' }}>
                                            <button
                                                onClick={() => improveEmail()}
                                                style={{
                                                    background: 'var(--color-background-info)',
                                                    color: 'var(--color-text-info)',
                                                    border: 'none',
                                                    borderRadius: 'var(--border-radius-md)',
                                                    padding: '10px 16px',
                                                    cursor: 'pointer',
                                                    fontSize: '14px',
                                                    fontWeight: '500',
                                                    display: 'flex',
                                                    alignItems: 'center',
                                                    gap: '6px'
                                                }}
                                            >
                                                <RefreshCw /> Improve
                                            </button>
                                            <button
                                                onClick={() => improveEmail('Make this email shorter and more concise.')}
                                                style={{
                                                    background: 'var(--color-background-secondary)',
                                                    color: 'var(--color-text-primary)',
                                                    border: '0.5px solid var(--color-border-secondary)',
                                                    borderRadius: 'var(--border-radius-md)',
                                                    padding: '10px 16px',
                                                    cursor: 'pointer',
                                                    fontSize: '14px',
                                                    fontWeight: '500'
                                                }}
                                            >
                                                Shorten
                                            </button>
                                            <button
                                                onClick={() => improveEmail('Write a reply to this email professionally.')}
                                                style={{
                                                    background: 'var(--color-background-secondary)',
                                                    color: 'var(--color-text-primary)',
                                                    border: '0.5px solid var(--color-border-secondary)',
                                                    borderRadius: 'var(--border-radius-md)',
                                                    padding: '10px 16px',
                                                    cursor: 'pointer',
                                                    fontSize: '14px',
                                                    fontWeight: '500'
                                                }}
                                            >
                                                Reply
                                            </button>
                                            <button
                                                onClick={() => {
                                                    navigator.clipboard.writeText(emailDraft);
                                                    alert('Copied!');
                                                }}
                                                style={{
                                                    background: 'var(--color-background-secondary)',
                                                    color: 'var(--color-text-primary)',
                                                    border: '0.5px solid var(--color-border-secondary)',
                                                    borderRadius: 'var(--border-radius-md)',
                                                    padding: '10px 16px',
                                                    cursor: 'pointer',
                                                    fontSize: '14px',
                                                    fontWeight: '500',
                                                    display: 'flex',
                                                    alignItems: 'center',
                                                    gap: '6px'
                                                }}
                                            >
                                                <Copy /> Copy
                                            </button>
                                        </div>
                                    </div>

                                    {emailHistory.length > 0 && (
                                        <div>
                                            <h3 style={{ fontSize: '16px', fontWeight: '600', marginTop: '0', marginBottom: '12px', color: 'var(--color-text-primary)' }}>
                                                History ({emailHistory.length})
                                            </h3>
                                            {emailHistory.map((entry, idx) => (
                                                <div key={idx} style={{
                                                    background: 'var(--color-background-secondary)',
                                                    border: '0.5px solid var(--color-border-tertiary)',
                                                    borderRadius: 'var(--border-radius-md)',
                                                    padding: '1rem',
                                                    marginBottom: '12px',
                                                    fontSize: '13px'
                                                }}>
                                                    <p style={{ fontSize: '12px', fontWeight: '600', color: 'var(--color-text-secondary)', margin: '0 0 8px' }}>
                                                        {entry.tone} version
                                                    </p>
                                                    <p style={{ color: 'var(--color-text-primary)', margin: '0', whiteSpace: 'pre-wrap' }}>
                                                        {entry.improved}
                                                    </p>
                                                </div>
                                            ))}
                                        </div>
                                    )}
                                </div>
                            )}

                            {/* FILES */}
                            {activeTab === 'files' && (
                                <div>
                                    <div style={{ marginBottom: '1.5rem' }}>
                                        <button
                                            onClick={() => fileInputRef.current?.click()}
                                            style={{
                                                background: 'var(--color-background-info)',
                                                color: 'var(--color-text-info)',
                                                border: 'none',
                                                borderRadius: 'var(--border-radius-md)',
                                                padding: '10px 16px',
                                                cursor: 'pointer',
                                                fontSize: '14px',
                                                fontWeight: '500',
                                                display: 'flex',
                                                alignItems: 'center',
                                                gap: '8px'
                                            }}
                                        >
                                            <Upload /> Upload File
                                        </button>
                                        <input
                                            ref={fileInputRef}
                                            type="file"
                                            multiple
                                            onChange={handleFileUpload}
                                            style={{ display: 'none' }}
                                            accept=".pdf,.xlsx,.xls,.docx,.doc,.png,.jpg,.jpeg,.gif"
                                        />
                                    </div>

                                    {uploadedFiles.length === 0 ? (
                                        <div style={{
                                            textAlign: 'center',
                                            padding: '3rem',
                                            color: 'var(--color-text-secondary)',
                                            background: 'var(--color-background-secondary)',
                                            borderRadius: 'var(--border-radius-lg)',
                                            border: '0.5px dashed var(--color-border-secondary)'
                                        }}>
                                            <p>No files uploaded yet. Click the upload button to get started.</p>
                                        </div>
                                    ) : (
                                        <div style={{
                                            display: 'grid',
                                            gridTemplateColumns: 'repeat(auto-fill, minmax(280px, 1fr))',
                                            gap: '1rem'
                                        }}>
                                            {uploadedFiles.map(file => (
                                                <div
                                                    key={file.id}
                                                    style={{
                                                        background: 'var(--color-background-primary)',
                                                        border: '0.5px solid var(--color-border-tertiary)',
                                                        borderRadius: 'var(--border-radius-lg)',
                                                        padding: '1rem',
                                                        display: 'flex',
                                                        flexDirection: 'column',
                                                        gap: '12px'
                                                    }}
                                                >
                                                    <div>
                                                        <p style={{ fontSize: '14px', fontWeight: '600', margin: '0 0 4px', color: 'var(--color-text-primary)' }}>
                                                            {file.name}
                                                        </p>
                                                        <div style={{ display: 'flex', gap: '12px', fontSize: '12px', color: 'var(--color-text-secondary)' }}>
                                                            <span>{file.type}</span>
                                                            <span>{file.size}</span>
                                                        </div>
                                                    </div>
                                                    <div style={{ fontSize: '12px', color: 'var(--color-text-tertiary)' }}>
                                                        Company: {file.company}
                                                    </div>
                                                    <div style={{ display: 'flex', gap: '8px' }}>
                                                        <button
                                                            style={{
                                                                flex: 1,
                                                                background: 'var(--color-background-secondary)',
                                                                border: '0.5px solid var(--color-border-secondary)',
                                                                color: 'var(--color-text-primary)',
                                                                borderRadius: 'var(--border-radius-md)',
                                                                padding: '8px',
                                                                cursor: 'pointer',
                                                                fontSize: '12px',
                                                                fontWeight: '500',
                                                                display: 'flex',
                                                                alignItems: 'center',
                                                                justifyContent: 'center',
                                                                gap: '4px'
                                                            }}
                                                        >
                                                            <Download /> Download
                                                        </button>
                                                        <button
                                                            onClick={() => deleteFile(file.id)}
                                                            style={{
                                                                background: 'transparent',
                                                                border: 'none',
                                                                color: 'var(--color-text-secondary)',
                                                                cursor: 'pointer',
                                                                fontSize: '16px',
                                                                padding: '0'
                                                            }}
                                                        >
                                                            <Trash2 />
                                                        </button>
                                                    </div>
                                                </div>
                                            ))}
                                        </div>
                                    )}
                                </div>
                            )}
                        </div>
                    </div>

                    {/* Lead Conversion Modal */}
                    {showLeadForm && (
                        <div style={{
                            position: 'fixed',
                            top: 0,
                            left: 0,
                            right: 0,
                            bottom: 0,
                            background: 'rgba(0,0,0,0.4)',
                            display: 'flex',
                            alignItems: 'center',
                            justifyContent: 'center',
                            zIndex: 1000
                        }}>
                            <div style={{
                                background: 'var(--color-background-primary)',
                                borderRadius: 'var(--border-radius-lg)',
                                padding: '2rem',
                                maxWidth: '500px',
                                width: '90%',
                                maxHeight: '90vh',
                                overflow: 'auto',
                                boxShadow: '0 20px 60px rgba(0,0,0,0.15)'
                            }}>
                                <h2 style={{ fontSize: '18px', fontWeight: '600', marginTop: '0', marginBottom: '1.5rem', color: 'var(--color-text-primary)' }}>
                                    Convert to Lead
                                </h2>

                                <div style={{ display: 'flex', flexDirection: 'column', gap: '1rem' }}>
                                    <div>
                                        <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                            Contact Name
                                        </label>
                                        <input
                                            type="text"
                                            value={leadForm.contactName}
                                            onChange={(e) => setLeadForm({ ...leadForm, contactName: e.target.value })}
                                            style={{
                                                width: '100%',
                                                padding: '10px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                fontSize: '14px',
                                                marginTop: '4px',
                                                boxSizing: 'border-box',
                                                background: 'var(--color-background-secondary)',
                                                color: 'var(--color-text-primary)',
                                                outline: 'none'
                                            }}
                                        />
                                    </div>

                                    <div>
                                        <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                            Insurance Scope
                                        </label>
                                        <select
                                            value={leadForm.insuranceScope}
                                            onChange={(e) => setLeadForm({ ...leadForm, insuranceScope: e.target.value })}
                                            style={{
                                                width: '100%',
                                                padding: '10px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                fontSize: '14px',
                                                marginTop: '4px',
                                                boxSizing: 'border-box',
                                                background: 'var(--color-background-secondary)',
                                                color: 'var(--color-text-primary)',
                                                outline: 'none'
                                            }}
                                        >
                                            <option>Life Insurance</option>
                                            <option>Health Insurance</option>
                                            <option>Life / Health</option>
                                            <option>Other</option>
                                        </select>
                                    </div>

                                    <div>
                                        <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                            Broker?
                                        </label>
                                        <select
                                            value={leadForm.broker}
                                            onChange={(e) => setLeadForm({ ...leadForm, broker: e.target.value })}
                                            style={{
                                                width: '100%',
                                                padding: '10px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                fontSize: '14px',
                                                marginTop: '4px',
                                                boxSizing: 'border-box',
                                                background: 'var(--color-background-secondary)',
                                                color: 'var(--color-text-primary)',
                                                outline: 'none'
                                            }}
                                        >
                                            <option value="No">No</option>
                                            <option value="Yes">Yes</option>
                                        </select>
                                    </div>

                                    {leadForm.broker === 'Yes' && (
                                        <div>
                                            <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                                Broker Name
                                            </label>
                                            <input
                                                type="text"
                                                value={leadForm.brokerName}
                                                onChange={(e) => setLeadForm({ ...leadForm, brokerName: e.target.value })}
                                                style={{
                                                    width: '100%',
                                                    padding: '10px',
                                                    border: '0.5px solid var(--color-border-secondary)',
                                                    borderRadius: 'var(--border-radius-md)',
                                                    fontSize: '14px',
                                                    marginTop: '4px',
                                                    boxSizing: 'border-box',
                                                    background: 'var(--color-background-secondary)',
                                                    color: 'var(--color-text-primary)',
                                                    outline: 'none'
                                                }}
                                                placeholder="e.g., ABC Brokers"
                                            />
                                        </div>
                                    )}

                                    <div>
                                        <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                            Renewal Date
                                        </label>
                                        <input
                                            type="date"
                                            value={leadForm.renewalDate}
                                            onChange={(e) => setLeadForm({ ...leadForm, renewalDate: e.target.value })}
                                            style={{
                                                width: '100%',
                                                padding: '10px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                fontSize: '14px',
                                                marginTop: '4px',
                                                boxSizing: 'border-box',
                                                background: 'var(--color-background-secondary)',
                                                color: 'var(--color-text-primary)',
                                                outline: 'none'
                                            }}
                                        />
                                    </div>

                                    <div>
                                        <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                            Expected Business
                                        </label>
                                        <select
                                            value={leadForm.expectedBusiness}
                                            onChange={(e) => setLeadForm({ ...leadForm, expectedBusiness: e.target.value })}
                                            style={{
                                                width: '100%',
                                                padding: '10px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                fontSize: '14px',
                                                marginTop: '4px',
                                                boxSizing: 'border-box',
                                                background: 'var(--color-background-secondary)',
                                                color: 'var(--color-text-primary)',
                                                outline: 'none'
                                            }}
                                        >
                                            <option>Health Insurance</option>
                                            <option>Life Insurance</option>
                                            <option>Both</option>
                                        </select>
                                    </div>

                                    <div>
                                        <label style={{ fontSize: '13px', fontWeight: '500', color: 'var(--color-text-primary)' }}>
                                            Next Steps
                                        </label>
                                        <select
                                            value={leadForm.nextSteps}
                                            onChange={(e) => setLeadForm({ ...leadForm, nextSteps: e.target.value })}
                                            style={{
                                                width: '100%',
                                                padding: '10px',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                fontSize: '14px',
                                                marginTop: '4px',
                                                boxSizing: 'border-box',
                                                background: 'var(--color-background-secondary)',
                                                color: 'var(--color-text-primary)',
                                                outline: 'none'
                                            }}
                                        >
                                            <option>Active List</option>
                                            <option>Meeting</option>
                                            <option>Follow-up</option>
                                        </select>
                                    </div>

                                    <div style={{ display: 'flex', gap: '12px', marginTop: '1rem' }}>
                                        <button
                                            onClick={() => {
                                                setShowLeadForm(false);
                                                setSelectedContact(null);
                                            }}
                                            style={{
                                                flex: 1,
                                                padding: '10px',
                                                background: 'var(--color-background-secondary)',
                                                border: '0.5px solid var(--color-border-secondary)',
                                                borderRadius: 'var(--border-radius-md)',
                                                cursor: 'pointer',
                                                fontSize: '14px',
                                                fontWeight: '500',
                                                color: 'var(--color-text-primary)'
                                            }}
                                        >
                                            Cancel
                                        </button>
                                        <button
                                            onClick={submitLeadForm}
                                            style={{
                                                flex: 1,
                                                padding: '10px',
                                                background: 'var(--color-background-success)',
                                                color: 'var(--color-text-success)',
                                                border: 'none',
                                                borderRadius: 'var(--border-radius-md)',
                                                cursor: 'pointer',
                                                fontSize: '14px',
                                                fontWeight: '500'
                                            }}
                                        >
                                            Convert to Lead
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    )}
                </div>
            );
        };

        ReactDOM.createRoot(document.getElementById('root')).render(<ModernCRM />);
    </script>
</body>
</html>
