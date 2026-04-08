# Contributing to Heimdahl-JS 🚀

Thank you for your interest in contributing to Heimdahl-JS! This document provides guidelines and instructions for contributing to our onchain data platform SDK.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Project Structure](#project-structure)
- [Contributing Workflow](#contributing-workflow)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Security](#security)
- [Community](#community)

## Code of Conduct

This project adheres to a code of conduct. By participating, you are expected to uphold this code:

- Be respectful and inclusive
- Welcome newcomers
- Focus on constructive feedback
- Respect different viewpoints and experiences

## Getting Started

### Prerequisites

- **Node.js** (v16 or higher)
- **npm** or **yarn**
- **Git**
- Familiarity with:
  - Blockchain data engineering
  - JavaScript/TypeScript
  - REST APIs
  - Multiple blockchain platforms (EVM and non-EVM)

### Supported Blockchains

Heimdahl-JS supports data retrieval from:
- **Base**
- **Ethereum**
- **Arbitrum**
- **Binance Smart Chain**
- **Polygon**
- **Solana**
- **Tron**
- And more!

### Quick Start

1. **Fork the repository** on GitHub
2. **Clone your fork**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/heimdahl-js.git
   cd heimdahl-js
   ```
3. **Install dependencies**:
   ```bash
   npm install
   ```
4. **Run tests**:
   ```bash
   npm test
   ```

## Development Setup

### Local Development

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Set up environment variables** (if needed):
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Run the development build**:
   ```bash
   npm run build
   ```

4. **Test the SDK**:
   ```bash
   node demo/example.js
   ```

### Testing Data Retrieval

Test the SDK against different blockchains:

```javascript
const Heimdahl = require('heimdahl-js');
const heimdahlClient = new Heimdahl();

// Test Ethereum
const ethData = await heimdahlClient.getData('ethereum', 'latestBlock');
console.log('Ethereum:', ethData);

// Test Base
const baseData = await heimdahlClient.getData('base', 'latestBlock');
console.log('Base:', baseData);

// Test Solana
const solData = await heimdahlClient.getData('solana', 'latestBlock');
console.log('Solana:', solData);
```

## Project Structure

```
heimdahl-js/
├── Readme.md              # User documentation
├── package.json           # Dependencies and scripts
├── .gitignore             # Git ignore rules
├── src/                   # Source code
│   ├── index.js           # Main SDK entry point
│   ├── client.js          # Heimdahl client implementation
│   ├── chains/            # Blockchain-specific adapters
│   │   ├── ethereum.js
│   │   ├── base.js
│   │   ├── solana.js
│   │   └── ...
│   └── utils/             # Helper functions
├── demo/                  # Example usage
│   └── example.js
├── tests/                 # Test suite
└── examples/              # Additional examples
```

### Key Components

- **Heimdahl Client**: Main SDK interface
- **Chain Adapters**: Blockchain-specific data retrieval
- **Utilities**: Helper functions for data processing
- **Demo**: Example implementations

## Contributing Workflow

### Branch Naming

- `feature/description` - New features
- `fix/description` - Bug fixes
- `docs/description` - Documentation updates
- `test/description` - Test additions/improvements
- `chain/description` - New blockchain support
- `refactor/description` - Code refactoring

Example: `feature/add-base-sepolia-support`

### Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `test`: Test changes
- `refactor`: Code refactoring
- `perf`: Performance improvements
- `chore`: Maintenance tasks

Examples:
```
feat(chains): add Base Sepolia support
fix(ethereum): resolve block number parsing error
docs(readme): update supported chains list
test(solana): add data retrieval tests
```

### Pull Request Process

1. **Create a branch** for your changes
2. **Make your changes** following our coding standards
3. **Test thoroughly** against multiple blockchains
4. **Update documentation** if needed
5. **Submit a pull request** with:
   - Clear title and description
   - Reference to any related issues
   - Test results
   - Example usage (if applicable)

### PR Review Criteria

- Code follows style guidelines
- Tests pass
- Documentation is updated
- Works across supported blockchains
- No breaking changes (or properly documented)

## Coding Standards

### JavaScript Style

- Use **ESLint** for linting
- Follow **Prettier** formatting
- Use **async/await** for asynchronous code
- Add **JSDoc comments** for public APIs

Example:
```javascript
/**
 * Retrieve onchain data for a specific blockchain
 * @param {string} chain - Blockchain identifier (ethereum, base, solana, etc.)
 * @param {string} query - Data query type (latestBlock, transactions, etc.)
 * @returns {Promise<Object>} Retrieved data
 */
async function getData(chain, query) {
  // Implementation
}
```

### Error Handling

Always handle errors gracefully:

```javascript
try {
  const data = await heimdahlClient.getData(chain, query);
  return data;
} catch (error) {
  if (error.code === 'UNSUPPORTED_CHAIN') {
    throw new Error(`Chain ${chain} is not supported`);
  }
  if (error.code === 'RATE_LIMIT') {
    // Implement retry logic
    return await retryWithBackoff(() => getData(chain, query));
  }
  throw error;
}
```

### Adding New Chain Support

When adding support for a new blockchain:

```javascript
// src/chains/newchain.js
class NewChainAdapter {
  constructor(config) {
    this.config = config;
  }

  async getLatestBlock() {
    // Implementation
  }

  async getTransactions(address) {
    // Implementation
  }
}

module.exports = NewChainAdapter;
```

## Testing

### Running Tests

```bash
# Run all tests
npm test

# Run specific test file
npm test -- ethereum.test.js

# Run with coverage
npm run test:coverage
```

### Test Structure

```javascript
describe('Heimdahl Client', () => {
  test('should retrieve Ethereum latest block', async () => {
    const client = new Heimdahl();
    const data = await client.getData('ethereum', 'latestBlock');
    
    expect(data).toBeDefined();
    expect(data.blockNumber).toBeGreaterThan(0);
  });
  
  test('should retrieve Base data', async () => {
    const client = new Heimdahl();
    const data = await client.getData('base', 'latestBlock');
    
    expect(data).toBeDefined();
    expect(data.chain).toBe('base');
  });
});
```

### Integration Testing

Test against live blockchains:

```bash
# Run integration tests
npm run test:integration
```

### Manual Testing Checklist

- [ ] Data retrieval works for all supported chains
- [ ] Error messages are clear and helpful
- [ ] Rate limiting is handled properly
- [ ] SDK works in Node.js environment
- [ ] Demo examples run successfully

## Security

### Reporting Vulnerabilities

If you discover a security vulnerability:

1. **DO NOT** open a public issue
2. Email security@heimdahl.io with details
3. Include steps to reproduce
4. Allow time for remediation before disclosure

### Security Best Practices

- Never commit API keys or credentials
- Use environment variables for sensitive data
- Validate all user inputs
- Use HTTPS for all API calls
- Implement rate limiting
- Follow OWASP guidelines

## Community

### Communication Channels

- **GitHub Issues**: Bug reports and feature requests
- **GitHub Discussions**: General questions and ideas
- **Email**: support@heimdahl.io

### Getting Help

- Check existing [issues](https://github.com/Hussain-663/heimdahl-js/issues)
- Read the [documentation](https://docs.heimdahl.io)
- Ask in GitHub Discussions

### Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Mentioned in release notes

## Resources

### Learning Materials

- [Base Documentation](https://docs.base.org)
- [Ethereum Documentation](https://ethereum.org/developers)
- [Solana Documentation](https://docs.solana.com)
- [Web3.js](https://web3js.readthedocs.io/)
- [Ethers.js](https://docs.ethers.io/)

### Tools

- [Node.js](https://nodejs.org/) - JavaScript runtime
- [Jest](https://jestjs.io/) - Testing framework
- [ESLint](https://eslint.org/) - Linting
- [Prettier](https://prettier.io/) - Code formatting

---

Thank you for contributing to Heimdahl-JS! Together we're building the future of onchain data access. 🚀

**Happy coding! 🎉**
