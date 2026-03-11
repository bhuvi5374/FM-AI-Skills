# Contributing to FM-AI-Skills

Thank you for your interest in contributing to the CloudBees Feature Management AI Skills repository! This guide will help you contribute safely and effectively.

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Security Guidelines](#security-guidelines)
- [Pull Request Process](#pull-request-process)
- [Content Guidelines](#content-guidelines)

## Code of Conduct

### Our Standards

We are committed to providing a welcoming and inclusive experience for everyone. We expect all contributors to:

- ✅ Use welcoming and inclusive language
- ✅ Be respectful of differing viewpoints and experiences
- ✅ Gracefully accept constructive criticism
- ✅ Focus on what is best for the community
- ✅ Show empathy towards other community members

### Unacceptable Behavior

- ❌ Harassment, trolling, or discriminatory comments
- ❌ Publishing others' private information
- ❌ Spam or excessive self-promotion
- ❌ Other conduct which could reasonably be considered inappropriate

## How to Contribute

### Types of Contributions We Welcome

1. **Journey Improvements**
   - Clarifying existing journey steps
   - Adding edge cases
   - Improving examples
   - Fixing typos or errors

2. **New Journeys**
   - Additional user workflows
   - Different use cases
   - Platform-specific variations

3. **Skill Definitions**
   - New skill files
   - Enhanced skill descriptions
   - Better examples

4. **Documentation**
   - README improvements
   - Better navigation
   - Clearer explanations

### Getting Started

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR-USERNAME/FM-AI-Skills.git
   cd FM-AI-Skills
   ```

3. **Create a branch**
   ```bash
   git checkout -b feature/your-improvement-name
   ```

4. **Make your changes**
   - Follow the existing file structure
   - Use the templates provided
   - Write clear commit messages

5. **Test your changes**
   - Review for typos and formatting
   - Ensure Markdown renders correctly
   - Verify links work

6. **Push to your fork**
   ```bash
   git push origin feature/your-improvement-name
   ```

7. **Create a Pull Request**
   - Go to the original repository
   - Click "New Pull Request"
   - Select your branch
   - Describe your changes clearly

## Security Guidelines

### 🔒 CRITICAL: Never Include Sensitive Information

**DO NOT commit:**
- ❌ API keys, tokens, or credentials
- ❌ Customer data or PII
- ❌ Internal CloudBees systems information
- ❌ Production configuration details
- ❌ Secrets or passwords
- ❌ Real email addresses (use examples)
- ❌ Real company names (use placeholders)
- ❌ Proprietary information

**Before committing, always check:**
```bash
# Review what you're about to commit
git diff

# Look for sensitive patterns
git diff | grep -i "password\|secret\|key\|token\|credential"
```

### ✅ Safe to Include

- ✅ Generic examples and placeholders
- ✅ Public documentation references
- ✅ Educational content
- ✅ Best practices and patterns
- ✅ Template structures

### Example Replacements

**DON'T:**
```markdown
API_KEY=sk-1234567890abcdef
email: john.doe@acme-corp.com
Database: prod-db-internal-01.cloudbees.com
```

**DO:**
```markdown
API_KEY=your-api-key-here
email: user@example.com
Database: your-database-host
```

## Pull Request Process

### Before Submitting

- [ ] Read the [SECURITY.md](SECURITY.md) policy
- [ ] Ensure no sensitive information is included
- [ ] Test that Markdown renders correctly
- [ ] Write clear commit messages
- [ ] Update relevant documentation

### PR Description Template

```markdown
## Summary
Brief description of your changes

## Type of Change
- [ ] Bug fix
- [ ] New journey
- [ ] Journey improvement
- [ ] Documentation update
- [ ] Other (describe)

## Changes Made
- List specific changes
- Be clear and concise

## Testing
- How did you verify your changes?
- What did you test?

## Checklist
- [ ] No sensitive information included
- [ ] Markdown renders correctly
- [ ] Links work properly
- [ ] Follows existing format
- [ ] Clear commit messages
```

### Review Process

1. **Automated Checks** - Basic validation runs automatically
2. **Maintainer Review** - Repository owner reviews for:
   - Content quality
   - Security compliance
   - Formatting consistency
   - Accuracy
3. **Feedback** - You may receive feedback or change requests
4. **Approval** - Once approved, changes will be merged
5. **Credit** - You'll be credited in the commit history

### Response Times

- **Acknowledgment:** Within 3-5 business days
- **Initial Review:** Within 1-2 weeks
- **Final Decision:** Within 2-4 weeks (depends on complexity)

## Content Guidelines

### Journey Files

Follow the template in `Unify AI Assistant/JOURNEY-TEMPLATE.md`:

- Clear, descriptive titles
- Realistic user scenarios
- Step-by-step workflows
- Required skills listed
- Edge cases covered
- Success metrics defined

### Skill Files

Follow the template in `Unify AI Assistant/SKILL-TEMPLATE.md`:

- Clear purpose statement
- Input/output specifications
- Detailed instructions
- Safety guardrails
- Multiple examples
- Testing checklist

### Writing Style

- **Clear and Concise** - No jargon unless necessary
- **Action-Oriented** - Use active voice
- **User-Focused** - Write from user's perspective
- **Example-Rich** - Include concrete examples
- **Safe Defaults** - Prioritize safety and security

### Formatting

- Use Markdown formatting consistently
- Code blocks with appropriate syntax highlighting
- Tables for structured data
- Lists for steps and options
- Headers for organization

## Questions?

If you have questions:

1. **Check existing issues:** https://github.com/bhuvi5374/FM-AI-Skills/issues
2. **Create a new issue:** Describe your question clearly
3. **Email:** bhuvanesh.g5374@gmail.com (for private questions)

## License

By contributing, you agree that your contributions will be available under the same terms as the repository.

---

**Thank you for contributing to FM-AI-Skills!** 🎉

Your contributions help make feature management more accessible and effective for everyone.
