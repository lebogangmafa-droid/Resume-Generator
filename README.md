// src/App.js
import React, { useState } from 'react';
import ResumeForm from './components/ResumeForm';
import ResumePreview from './components/ResumePreview';
import SuggestionsPanel from './components/SuggestionsPanel';
import { industrySuggestions } from './data/industryData';
import { useResumeData } from './hooks/useResumeData';
import './styles.css';

function App() {
  const {
    resumeContent,
    updateResumeContent,
    jobDescription,
    setJobDescription,
    industry,
    setIndustry,
    template,
    setTemplate,
    suggestions,
    setSuggestions,
    feedback,
    saveFeedback
  } = useResumeData();

  const [activeTab, setActiveTab] = useState("editor");

  const applySuggestions = () => {
    setSuggestions(industrySuggestions[industry] || []);
  };

  return (
    <div className="resume-generator-app">
      <header className="app-header">
        <h1>Intelligent ATS Resume Generator</h1>
        <p>Create a job-winning resume optimized for Applicant Tracking Systems</p>
      </header>

      <div className="app-tabs">
        <button 
          className={activeTab === "editor" ? "active" : ""}
          onClick={() => setActiveTab("editor")}
        >
          Editor
        </button>
        <button 
          className={activeTab === "preview" ? "active" : ""}
          onClick={() => setActiveTab("preview")}
        >
          Preview
        </button>
      </div>

      <div className="app-content">
        {activeTab === "editor" ? (
          <div className="editor-container">
            <div className="form-section">
              <ResumeForm
                industry={industry}
                setIndustry={setIndustry}
                jobDescription={jobDescription}
                setJobDescription={setJobDescription}
                resumeContent={resumeContent}
                updateResumeContent={updateResumeContent}
                applySuggestions={applySuggestions}
              />
            </div>
            
            <div className="suggestions-section">
              <SuggestionsPanel 
                suggestions={suggestions}
                industry={industry}
              />
            </div>
          </div>
        ) : (
          <div className="preview-section">
            <ResumePreview 
              resumeContent={resumeContent}
              template={template}
              setTemplate={setTemplate}
            />
          </div>
        )}
      </div>

      <footer className="app-footer">
        <button onClick={saveFeedback} className="save-button">
          Save Resume
        </button>
        <p className="footer-note">
          Your data is stored locally in your browser and is not sent to any server.
        </p>
      </footer>
    </div>
  );
}

export default App;
