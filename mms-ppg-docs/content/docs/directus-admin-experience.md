---
title: 'Directus Admin Guide'
description: 'How to administer Directus'
---
# Directus Admin Experience for MMS PPG Website

## Overview

This document describes the comprehensive Directus admin experience configured for the MMS PPG website, featuring PPG branding, content workflows, role-based permissions, and advanced collaboration tools.

## 🎨 PPG Branding & Theme

### Brand Colors
The admin interface uses the official PPG brand colors derived from the logo:

- **Primary Green**: `#7CB342` - Main brand color for buttons and highlights
- **Secondary Green**: `#4CAF50` - Supporting elements and hover states
- **Accent Green**: `#2E7D32` - Navigation and focus indicators
- **Supporting Green**: `#A5D6A7` - Backgrounds and subtle elements
- **Neutral Green**: `#66BB6A` - Interactive elements
- **Warm Accent**: `#FF8A65` - Call-to-action elements
- **Cool Accent**: `#42A5F5` - Informational elements

### Visual Identity
- Custom CSS with PPG color scheme
- Accessibility-compliant focus indicators
- High contrast mode support
- Reduced motion preferences respected
- Consistent typography and spacing

### Admin Interface Customizations
- PPG logo in header
- Custom status indicators with brand colors
- Enhanced accessibility features
- Responsive design for all devices

## 👥 Role-Based Access Control

### Content Creator Role
**Permissions:**
- Create and edit own content (blog posts, events)
- Read access to all published content
- Upload and manage own files
- Submit content for review
- Cannot publish or delete published content

**Workflow:**
1. Create content in draft status
2. Edit and refine content
3. Submit for review (status: `in_review`)
4. Receive feedback from reviewers
5. Make revisions as needed

### Content Reviewer Role
**Permissions:**
- Read access to all content (including drafts)
- Edit any content for review purposes
- Approve or reject content submissions
- Add review comments and feedback
- Cannot publish content directly

**Workflow:**
1. Receive notifications of content ready for review
2. Review content for quality, accuracy, and compliance
3. Provide feedback and suggestions
4. Approve content (status: `approved`) or request changes
5. Track review history and decisions

### Content Publisher Role
**Permissions:**
- Full access to all content collections
- Publish approved content
- Manage site settings and navigation
- Schedule content for future publication
- Delete content when necessary
- Manage user accounts and permissions

**Workflow:**
1. Review approved content
2. Schedule publication dates
3. Publish content to live website
4. Monitor content performance
5. Archive outdated content

### Site Administrator Role
**Permissions:**
- Full administrative access
- System configuration and settings
- User management and role assignment
- Backup and maintenance operations
- Security and performance monitoring

## 🔄 Content Workflows

### Draft → Review → Publish Workflow

#### 1. Content Creation
- All new content starts in `draft` status
- Creators can edit and refine content
- Auto-save functionality prevents data loss
- Version history tracks all changes

#### 2. Review Process
- Content submitted for review (status: `in_review`)
- Automatic notifications sent to reviewers
- Review records created with timestamps
- Comments and feedback system
- Approval or rejection with reasons

#### 3. Publication
- Approved content can be scheduled for publication
- Automatic publishing at scheduled times
- Website rebuild triggered on publication
- SEO metadata automatically generated

#### 4. Content Lifecycle
- Published content remains editable
- Changes require re-approval for major updates
- Archive functionality for outdated content
- Version control maintains content history

### Automated Workflows

#### Content Review Notifications
- Email notifications to reviewers
- In-app notifications and alerts
- Slack/Teams integration (optional)
- Escalation for overdue reviews

#### Scheduled Publishing
- Content scheduled for future publication
- Automatic status updates
- Website regeneration triggers
- Social media integration (optional)

#### Backup and Versioning
- Daily automated backups
- Content version history
- Change tracking and attribution
- Recovery and rollback capabilities

## 🖼️ Image Optimization Pipeline

### Automatic Transformations
When images are uploaded, the system automatically generates:

#### Thumbnail Variants
- **Small**: 150×150px, cover fit, 80% quality
- **Medium**: 300×300px, cover fit, 85% quality
- **Large**: 500×500px, cover fit, 85% quality

#### Content Variants
- **Small**: 400px width, contain fit, 80% quality
- **Medium**: 800px width, contain fit, 85% quality
- **Large**: 1200px width, contain fit, 85% quality

#### Hero Image Variants
- **Mobile**: 768×400px, cover fit, 85% quality
- **Tablet**: 1024×500px, cover fit, 85% quality
- **Desktop**: 1920×600px, cover fit, 90% quality

#### Social Media Variants
- **Open Graph**: 1200×630px, cover fit, 90% quality
- **Twitter Card**: 1200×600px, cover fit, 90% quality

#### Modern Format Support
- **WebP variants** for all sizes (better compression)
- **AVIF support** (future-ready format)
- **Fallback formats** for older browsers

### Image Management Features
- Drag-and-drop upload interface
- Bulk upload and processing
- Alt text and caption management
- Usage tracking across content
- Automatic optimization settings
- CDN integration for fast delivery

## 🤝 Real-Time Collaboration

### Live Editing Features
- **Real-time cursors**: See where other users are editing
- **Live selections**: View what others have selected
- **Conflict resolution**: Automatic handling of simultaneous edits
- **Content locking**: Prevent conflicts during editing
- **Activity indicators**: Show who's currently active

### Communication Tools
- **Comments system**: Add comments to specific content sections
- **Mentions**: @mention other users for attention
- **Review discussions**: Threaded conversations on content
- **Status updates**: Real-time workflow status changes

### Collaboration Settings
- **WebSocket connections**: Real-time updates
- **Heartbeat monitoring**: Connection health tracking
- **Automatic reconnection**: Seamless experience during network issues
- **Activity tracking**: Comprehensive audit trail

## 💾 Backup & Versioning Systems

### Content Versioning
- **Automatic versioning**: Every save creates a new version
- **Version comparison**: Side-by-side diff views
- **Rollback capability**: Restore previous versions
- **Change attribution**: Track who made what changes
- **Version limits**: Configurable retention (default: 10 versions)

### Backup Systems
- **Daily automated backups**: Complete content export
- **Incremental backups**: Changed content only
- **File inclusion**: Images and documents included
- **Retention policy**: 30-day default retention
- **Manual backups**: On-demand backup creation
- **Restore functionality**: Point-in-time recovery

### Data Export Options
- **JSON format**: Complete structured data
- **CSV format**: Tabular data for analysis
- **XML format**: Standards-compliant export
- **File packages**: Content with associated media
- **Selective export**: Choose specific collections

## 📊 Analytics & Monitoring

### Content Analytics
- **View tracking**: Page and content popularity
- **Engagement metrics**: Time spent, bounce rates
- **Search analytics**: Popular search terms
- **User behavior**: Navigation patterns
- **Performance metrics**: Load times, errors

### System Health Monitoring
- **Uptime tracking**: Service availability
- **Performance monitoring**: Response times
- **Error tracking**: System errors and warnings
- **Resource usage**: Memory, CPU, storage
- **User activity**: Active sessions, login patterns

### Dashboard Widgets
- **Content overview**: Total posts, pages, events
- **Recent activity**: Latest changes and updates
- **Review queue**: Content awaiting review
- **Upcoming events**: Scheduled content and events
- **System status**: Health indicators and alerts

## 🔧 Setup & Configuration

### Initial Setup
1. **Run setup script**: `npm run setup:directus-admin`
2. **Configure environment**: Set DIRECTUS_TOKEN in .env.local
3. **Verify setup**: Check admin panel for PPG branding
4. **Create users**: Add team members with appropriate roles
5. **Test workflows**: Create sample content and test approval process

### Environment Variables
```bash
# Required
DIRECTUS_URL=https://mmsppg.themainhost.co.uk
DIRECTUS_TOKEN=your-admin-token-here

# Optional - Webhook integration
WEBSITE_WEBHOOK_URL=https://mmsppg.org.uk
REVALIDATE_TOKEN=your-revalidate-token

# Email notifications
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=noreply@mmsppg.org.uk
SMTP_PASS=your-email-password
```

### Maintenance Tasks
- **Weekly**: Review backup integrity
- **Monthly**: Clean up old versions and logs
- **Quarterly**: Review user permissions and roles
- **Annually**: Update security settings and passwords

## 🚀 Getting Started

### For Content Creators
1. **Login**: Access admin panel at `/admin`
2. **Create content**: Use the content creation interface
3. **Add media**: Upload images with proper alt text
4. **Submit for review**: Change status to "In Review"
5. **Respond to feedback**: Make requested changes

### For Content Reviewers
1. **Check notifications**: Review pending content alerts
2. **Review content**: Check for accuracy and quality
3. **Provide feedback**: Add comments and suggestions
4. **Approve/reject**: Update content status accordingly
5. **Track progress**: Monitor review queue

### For Publishers
1. **Review approved content**: Final quality check
2. **Schedule publication**: Set publication dates
3. **Monitor performance**: Check analytics and metrics
4. **Manage site settings**: Update navigation and settings
5. **Maintain system**: Perform regular maintenance tasks

## 📚 Resources

### Documentation Links
- [Directus Documentation](https://docs.directus.io/)
- [PPG Brand Guidelines](./ppg-brand-guidelines.md)
- [Content Style Guide](./content-style-guide.md)
- [Accessibility Guidelines](./accessibility-guidelines.md)

### Support Contacts
- **Technical Issues**: developer@mmsppg.org.uk
- **Content Questions**: content@mmsppg.org.uk
- **Admin Access**: admin@mmsppg.org.uk

### Training Materials
- **Video Tutorials**: Available in admin panel help section
- **User Guides**: Step-by-step instructions for common tasks
- **Best Practices**: Guidelines for effective content management
- **Troubleshooting**: Common issues and solutions

## 🔒 Security & Compliance

### Security Features
- **Two-factor authentication**: Required for publishers and admins
- **IP restrictions**: Limit access by location (optional)
- **Session management**: Automatic logout and session limits
- **Audit logging**: Complete activity tracking
- **Data encryption**: All data encrypted in transit and at rest

### Compliance Considerations
- **GDPR compliance**: Data protection and user rights
- **Accessibility standards**: WCAG 2.1 AA compliance
- **Content policies**: Editorial guidelines and standards
- **Privacy protection**: No patient data in content management

### Backup & Recovery
- **Regular backups**: Automated daily backups
- **Disaster recovery**: Documented recovery procedures
- **Data retention**: Configurable retention policies
- **Security monitoring**: Continuous threat detection

This comprehensive admin experience ensures that the MMS PPG website content management is efficient, secure, and user-friendly while maintaining the highest standards of accessibility and brand consistency.