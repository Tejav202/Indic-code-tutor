# Design Document: IndicCode Tutor

## Overview

IndicCode Tutor is an AI-powered educational platform that helps Indian students learn programming by providing code explanations, error detection, and tutoring in their native languages. The system bridges the language barrier for students in Tier-3 colleges and regional areas by translating complex programming concepts into familiar Indian languages with cultural context.

## Technical Architecture

### High-Level Architecture

The system follows a serverless, cloud-native architecture optimized for cost-effectiveness and scalability:

```
[React Frontend] → [API Gateway] → [Lambda Functions] → [AWS Bedrock]
                                ↓
                           [Session Storage]
```

### Core Components

#### Frontend Layer
- **Technology**: React.js with Tailwind CSS
- **Rationale**: React provides component reusability and efficient state management, while Tailwind ensures consistent, responsive design
- **Key Features**:
  - Code editor with syntax highlighting
  - Language selection dropdown
  - Interactive line-by-line explanation display
  - File upload and drag-drop functionality
  - Dark/light theme support

#### API Layer
- **Technology**: Amazon API Gateway
- **Rationale**: Provides managed API endpoints with built-in throttling, authentication, and monitoring
- **Endpoints**:
  - `POST /analyze-code` - Main code analysis endpoint
  - `POST /explain-line` - Interactive line explanation
  - `GET /supported-languages` - Available language list
  - `POST /detect-errors` - Error detection and explanation

#### Compute Layer
- **Technology**: AWS Lambda
- **Rationale**: Serverless architecture eliminates infrastructure management and provides automatic scaling with pay-per-use pricing
- **Functions**:
  - **Code Analyzer Lambda**: Processes uploaded code and validates syntax
  - **AI Explainer Lambda**: Interfaces with AWS Bedrock for explanations
  - **Error Detector Lambda**: Identifies syntax and logical errors
  - **Language Translator Lambda**: Handles multi-language translation

#### AI Engine
- **Technology**: AWS Bedrock (Claude-3 or Llama-3 models)
- **Rationale**: Managed AI service with high-quality language models, eliminating model hosting complexity
- **Capabilities**:
  - Code explanation generation
  - Error analysis and suggestions
  - Cultural context adaptation
  - Multi-language translation

## Detailed Component Design

### 1. File Processing System

**Component**: File_Processor
**Responsibilities**:
- Accept file uploads (.py, .js, .java) up to 5MB
- Validate file content and format
- Parse code into analyzable segments
- Handle direct code paste (up to 10,000 characters)

**Implementation**:
```javascript
// Frontend file validation
const validateFile = (file) => {
  const allowedTypes = ['.py', '.js', '.java'];
  const maxSize = 5 * 1024 * 1024; // 5MB
  return allowedTypes.includes(getFileExtension(file.name)) && file.size <= maxSize;
};
```

### 2. Multi-Language Support System

**Component**: Language_Translator
**Supported Languages**: Hindi, Telugu, Tamil, Bengali, Marathi
**Implementation Strategy**:
- Language preference stored in session state
- Default to Hindi if no selection made
- Display languages in both English and native scripts
- Persist selection throughout user session

**Language Configuration**:
```javascript
const supportedLanguages = {
  'hi': { name: 'Hindi', nativeName: 'हिंदी', default: true },
  'te': { name: 'Telugu', nativeName: 'తెలుగు' },
  'ta': { name: 'Tamil', nativeName: 'தமிழ்' },
  'bn': { name: 'Bengali', nativeName: 'বাংলা' },
  'mr': { name: 'Marathi', nativeName: 'मराठी' }
};
```

### 3. AI-Powered Explanation Engine

**Component**: AI_Explainer
**Core Features**:
- Line-by-line code explanation
- Beginner-friendly language adaptation
- Cultural context integration
- Concept-focused explanations over syntax translation

**Prompt Engineering Strategy**:
```
System: You are an expert programming tutor for Indian students. Explain code in {language} using:
1. Simple, beginner-friendly terms
2. Real-world analogies familiar to Indian students
3. Cultural context and examples
4. Focus on concepts, not just syntax
5. Progressive complexity based on code difficulty
```

### 4. Error Detection and Analysis

**Component**: Error_Detector
**Detection Capabilities**:
- Syntax error identification with line numbers
- Logical error detection (infinite loops, unreachable code)
- Variable usage analysis
- Best practice violations

**Error Prioritization**:
1. Syntax errors (blocking execution)
2. Runtime errors (causing crashes)
3. Logical errors (incorrect behavior)
4. Style and best practice issues

### 5. Interactive Code Analysis

**Features**:
- Click-to-explain functionality for individual lines
- Related line highlighting for dependencies
- Multi-operation breakdown for complex statements
- Context-aware explanations

**Implementation**:
```javascript
const handleLineClick = async (lineNumber, code) => {
  const explanation = await explainSpecificLine({
    code,
    lineNumber,
    language: selectedLanguage,
    context: getLineContext(code, lineNumber)
  });
  setActiveExplanation({ lineNumber, explanation });
};
```

### 6. Session Management

**Features**:
- Language preference persistence
- Multiple file analysis support
- Session-based code storage
- Privacy-focused data clearing

**Session Structure**:
```javascript
const sessionState = {
  userId: generateSessionId(),
  selectedLanguage: 'hi',
  analyzedFiles: [],
  preferences: {},
  createdAt: timestamp,
  expiresAt: timestamp + sessionDuration
};
```

## Data Flow Architecture

### Code Analysis Flow
1. User uploads file or pastes code
2. Frontend validates input and sends to API Gateway
3. Code Analyzer Lambda processes and validates code
4. AI Explainer Lambda generates explanations via Bedrock
5. Language Translator Lambda adapts content to selected language
6. Results returned to frontend for display

### Error Detection Flow
1. Code submitted for analysis
2. Error Detector Lambda performs syntax and logical analysis
3. Errors prioritized and categorized
4. AI Explainer generates error explanations and fixes
5. Translated explanations returned to user

## Security and Privacy Design

### Data Protection
- No persistent storage of user code
- Session-based temporary storage only
- Automatic data cleanup on session end
- HTTPS encryption for all communications

### AWS Security
- IAM roles with minimal required permissions
- API Gateway throttling and rate limiting
- Lambda function isolation
- Bedrock API key management through AWS Secrets Manager

## Performance Optimization

### Frontend Optimization
- Code splitting for faster initial load
- Lazy loading of explanation components
- Debounced API calls for interactive features
- Caching of language preferences

### Backend Optimization
- Lambda cold start mitigation through provisioned concurrency
- Bedrock API call optimization and batching
- Response caching for common explanations
- Efficient error handling and retry mechanisms

## Scalability Considerations

### Auto-scaling Strategy
- Lambda functions scale automatically based on demand
- API Gateway handles traffic spikes with built-in throttling
- Bedrock provides managed scaling for AI processing
- CloudFront CDN for global content delivery

### Cost Optimization
- Pay-per-use Lambda pricing model
- Bedrock usage optimization through prompt engineering
- Efficient API call patterns to minimize costs
- Session management to prevent resource waste

## User Experience Design

### Interface Design Principles
- Clean, intuitive layout optimized for students
- Clear visual separation between code and explanations
- Readable fonts and appropriate contrast ratios
- Responsive design for various screen sizes

### Accessibility Features
- Keyboard navigation support
- Screen reader compatibility
- High contrast mode option
- Font size adjustment capabilities

## Error Handling and Resilience

### Frontend Error Handling
- Graceful degradation for network issues
- User-friendly error messages in selected language
- Retry mechanisms for failed requests
- Offline capability indicators

### Backend Error Handling
- Comprehensive error logging and monitoring
- Fallback mechanisms for Bedrock unavailability
- Circuit breaker patterns for external dependencies
- Graceful timeout handling

## Monitoring and Analytics

### Performance Monitoring
- Lambda function performance metrics
- API Gateway request/response monitoring
- Bedrock usage and latency tracking
- Frontend performance analytics

### User Analytics
- Language preference distribution
- Feature usage patterns
- Error frequency analysis
- Session duration and engagement metrics

## Deployment Strategy

### Infrastructure as Code
- AWS CDK for infrastructure provisioning
- Environment-specific configurations
- Automated deployment pipelines
- Blue-green deployment for zero downtime

### CI/CD Pipeline
1. Code commit triggers build process
2. Automated testing (unit, integration, e2e)
3. Infrastructure deployment via CDK
4. Application deployment to Lambda
5. Health checks and rollback capabilities

## Future Enhancements

### Planned Features
- Additional programming language support (C++, C#)
- Advanced code quality metrics
- Personalized learning paths
- Integration with popular IDEs
- Mobile application development

### Scalability Roadmap
- Multi-region deployment for global reach
- Advanced caching strategies
- Machine learning model fine-tuning
- Enhanced cultural context adaptation


