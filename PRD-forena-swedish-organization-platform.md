# PRD: Förena - Open-Source Swedish Organization Management Platform

## Problem Statement

Swedish sports clubs, housing cooperatives, hobby clubs, and community organizations currently rely on proprietary platforms (Boappa, Samfälligheten, Sportadmin, Laget.se) that suffer from poor usability, high costs, vendor lock-in, and lack of transparency. These platforms often require multiple clicks for simple tasks, have confusing interfaces for non-technical volunteers, and provide no visibility into how user data is processed or stored. Swedish organizations need a modern, transparent, and highly usable alternative that respects their privacy and provides full control over their data.

## Solution

Förena is an open-source, self-hostable organization management platform specifically designed for Swedish sports clubs, housing cooperatives, and community organizations. Built with a mobile-first approach and Swedish UX design principles, Förena provides streamlined workflows, smart defaults, and automated features that make organization management simple for non-technical volunteers. The platform offers complete transparency through open-source code, full data ownership through self-hosting, and compliance with Swedish privacy requirements.

## User Stories

### Member/Resident Management
1. As a sports club administrator, I want to register new players with their personnummer and parent contact information, so that I can maintain accurate team rosters
2. As an HOA board member, I want to track which unit belongs to which resident, so that I can send communications to the right people
3. As a community group leader, I want to see member profiles with custom fields for interests and skills, so that I can match volunteers with appropriate tasks
4. As a member, I want to update my own contact information and privacy preferences, so that I control how the organization can reach me
5. As an administrator, I want to export member lists with GDPR compliance, so that I can provide data to members upon request
6. As a parent, I want to manage multiple children's memberships from one account, so that I don't need separate logins for each child

### Event & Scheduling Management  
7. As a coach, I want to schedule recurring training sessions that automatically account for Swedish holidays, so that players know when practice is cancelled
8. As an HOA board secretary, I want to schedule board meetings with automatic protocol templates, so that meeting minutes follow proper format
9. As a member, I want to RSVP to events from my mobile phone with one tap, so that I can quickly confirm attendance
10. As an event organizer, I want to set capacity limits and manage waitlists, so that popular events don't exceed venue limits
11. As a sports club member, I want to see match schedules integrated with league data, so that I know game times and locations
12. As an administrator, I want to send automatic event reminders, so that attendance rates improve without manual work

### Resource & Facility Booking
13. As a sports club member, I want to book training facilities and equipment, so that my team has dedicated practice time
14. As an HOA resident, I want to reserve common areas like laundry rooms and meeting spaces, so that I can use shared facilities without conflicts
15. As a facility manager, I want to see booking conflicts highlighted automatically, so that double-bookings are prevented
16. As a member, I want to receive booking confirmations via SMS or email, so that I have proof of my reservation
17. As an administrator, I want to set booking rules and restrictions per resource, so that facilities are used appropriately
18. As a user, I want to cancel bookings with appropriate notice, so that others can use the time slot

### Communication & Authority Signaling
19. As a board member, I want my messages to be visually distinct from regular member messages, so that important communications stand out
20. As an HOA administrator, I want to send announcements to specific buildings or units, so that irrelevant residents aren't bothered
21. As a sports team captain, I want to send quick updates to my team, so that players stay informed about changes
22. As a member, I want to control notification preferences for different types of messages, so that I only receive relevant communications
23. As a community group leader, I want to create message templates for common announcements, so that communication is consistent and professional
24. As a resident, I want to easily identify which messages are from official board sources versus informal neighbor communications, so that I can prioritize appropriately

### Workflow Management (HOA-Specific)
25. As an HOA resident, I want to submit maintenance requests with photos and descriptions, so that building issues are tracked properly
26. As a board member, I want to track maintenance request status and assign them to contractors, so that nothing falls through the cracks
27. As an HOA member, I want to participate in building project voting, so that I have a voice in major decisions
28. As a board secretary, I want voting results automatically compiled and recorded, so that decisions are properly documented
29. As a property manager, I want to see maintenance history for each unit and common area, so that I can plan preventive maintenance
30. As a resident, I want to receive updates when my maintenance requests are addressed, so that I know when issues are resolved

### Workflow Management (Sports-Specific)
31. As a volunteer coordinator, I want to schedule referees and officials for matches, so that games can proceed as planned
32. As a coach, I want to track player attendance at training sessions, so that I can identify commitment levels
33. As a sports club administrator, I want to manage equipment inventory and maintenance, so that gear is always ready for use
34. As a team manager, I want to coordinate volunteer schedules for tournaments, so that all roles are covered
35. As a league coordinator, I want to automatically update standings and statistics, so that results are immediately available

### Analytics & Audit Trail
36. As an administrator, I want to see resource usage graphs over time, so that I can optimize facility scheduling
37. As an HOA board member, I want to track long-term maintenance costs per building area, so that I can budget appropriately
38. As a sports club treasurer, I want to see member participation trends, so that I can adjust programming to meet demand
39. As a compliance officer, I want to access audit logs of all data access and changes, so that I can demonstrate GDPR compliance
40. As a board member, I want to generate annual activity reports automatically, so that I can present year-end summaries
41. As a member, I want to see my own activity history and data usage, so that I understand what information the organization has about me

### Swedish-Specific Features
42. As a Swedish user, I want to authenticate using my personnummer, so that my identity is verified securely
43. As an administrator, I want Swedish holiday calendars automatically integrated, so that events respect national holidays
44. As a member, I want all interfaces in Swedish with appropriate cultural context, so that the system feels native
45. As a data controller, I want GDPR compliance built into the system with Swedish privacy interpretations, so that legal requirements are automatically met
46. As a user, I want addresses formatted according to Swedish postal standards, so that location data is correctly structured

### Mobile & Usability Features
47. As a busy volunteer, I want key tasks accessible with single taps on mobile, so that I can manage organization duties while on the go
48. As a non-technical board member, I want clear onboarding that explains each feature simply, so that I can use the system without IT support
49. As a new administrator, I want smart defaults for common organization types, so that setup is quick and follows best practices
50. As a member, I want the most common actions prominently displayed, so that I don't waste time searching through menus
51. As a user, I want consistent navigation patterns across all features, so that learning one area helps me understand others
52. As an occasional user, I want helpful tooltips and guidance, so that I remember how to use features between visits

## Implementation Decisions

### Architecture
- **Single-tenant deployment model**: Each organization gets dedicated database and API deployment for data isolation and customization
- **Event-driven architecture**: RabbitMQ-based event bus for decoupled communication between modules
- **Database-per-tenant**: PostgreSQL instance per organization with Drizzle ORM for type-safe database operations
- **GraphQL API**: Pothos-based code-first GraphQL schema for flexible client queries
- **Microservices approach**: Separate packages for API, member portal, public site, with shared schema and UI components

### Core Modules
- **Swedish Identity Module**: Personnummer handling, GDPR compliance, Swedish privacy rules, authentication integration points
- **Organization Configuration Module**: Multi-type support (sports/HOA/community), Swedish holiday integration, smart defaults per org type
- **Member/Unit Management Module**: Flexible member data model with custom fields, family relationships, unit assignments for HOAs
- **Resource Booking Module**: Facility and equipment scheduling with conflict detection and automated notifications
- **Event & Scheduling Module**: Recurring event support, Swedish calendar integration, multiple event types (training/meetings/matches)
- **Communication & Authority Module**: Message hierarchy and visual authority indicators, targeted communication by group/role
- **Workflow Management Module**: State machine-driven processes for maintenance requests, voting, task tracking
- **Audit & Analytics Module**: Comprehensive logging, usage analytics, GDPR compliance reporting

### Technical Stack
- **Frontend**: Next.js with mobile-first responsive design, Swedish UX patterns
- **Backend**: GraphQL Yoga with Pothos schema builder for type safety
- **Database**: PostgreSQL with Drizzle ORM for migrations and type-safe queries
- **Queue System**: RabbitMQ for event-driven communication between modules
- **Cache Layer**: Redis for session management and performance optimization
- **File Storage**: S3-compatible storage for documents and images
- **Authentication**: OIDC-compatible with potential BankID integration pathway

### Swedish Compliance
- **Data Privacy**: Built-in GDPR compliance with Swedish interpretations and member data export capabilities
- **Address Formatting**: Swedish postal code validation and address formatting standards
- **Holiday Integration**: Swedish national and regional holiday calendar integration
- **Language Support**: Native Swedish interfaces with cultural context, English fallback for technical terms

### Mobile & Usability Design
- **Mobile-First Approach**: Primary development target is mobile devices for volunteer accessibility
- **One-Tap Actions**: Critical functions (RSVP, booking, messaging) accessible with single interactions
- **Smart Defaults**: Organization type detection with pre-configured templates and workflows
- **Progressive Disclosure**: Advanced features hidden until needed, simple workflows prominent
- **Contextual Help**: Inline guidance and tooltips for occasional users

## Testing Decisions

### Testing Philosophy
Good tests focus on external behavior and user-facing functionality rather than internal implementation details. Tests should verify that the system behaves correctly from the user's perspective and maintains data integrity under various scenarios.

### Comprehensive Testing Coverage
All modules require extensive testing due to the high-quality, high-usability positioning:

- **Member/Unit Management Module**: Data integrity, personnummer validation, family relationship handling
- **Resource Booking Module**: Double-booking prevention, capacity management, time conflict detection
- **Communication Authority Module**: Message hierarchy display, notification routing, permission-based visibility
- **Event Scheduling Module**: Recurring event logic, Swedish holiday integration, timezone handling
- **Workflow Management Module**: State machine transitions, approval processes, deadline tracking
- **Swedish Identity Module**: Personnummer format validation, GDPR compliance workflows
- **Audit & Analytics Module**: Data accuracy in reports, log completeness, export functionality

### Testing Approach
- **Integration Tests**: Focus on end-to-end workflows that users actually perform (event creation → notification → RSVP)
- **API Tests**: Comprehensive GraphQL query and mutation testing with various user roles and permissions
- **Database Tests**: Data consistency, constraint enforcement, migration safety
- **Mobile UI Tests**: Touch interaction patterns, responsive behavior, performance on slower devices

### Prior Art in Codebase
Since this is a greenfield project, testing patterns should be established early:
- Vitest for unit and integration testing with TypeScript support
- Playwright for end-to-end user workflow testing
- Database testing with transaction rollback for isolation
- GraphQL testing with schema validation and permission checking

## Out of Scope

### Initial MVP Limitations
- **Payment Processing**: No integrated billing or payment collection (external integration point for future)
- **Advanced Financial Management**: No accounting features, invoicing, or detailed financial reporting
- **Third-Party Integrations**: No initial integrations with external calendar systems, accounting software, or marketing tools
- **Multi-Language Support**: Swedish and English only, no additional languages
- **Advanced Analytics**: Basic reporting only, no business intelligence or predictive analytics
- **Mobile Apps**: Web-based mobile interface only, no native iOS/Android applications

### Deferred Features
- **BankID Integration**: Authentication framework ready but integration deferred pending security review
- **Advanced Automation**: Basic automated notifications only, no complex workflow automation
- **API Rate Limiting**: Basic protection only, no sophisticated API management
- **Advanced Permissions**: Role-based access control only, no attribute-based or dynamic permissions
- **Real-time Collaboration**: Basic messaging only, no live document editing or real-time updates
- **Advanced Search**: Basic filtering only, no full-text search or advanced query capabilities

### Explicitly Excluded
- **Multi-tenant SaaS Operation**: Single-tenant self-hosted only (separate cloud platform handles multi-tenancy)
- **Enterprise Features**: No enterprise-specific features like SSO federation, advanced compliance, or dedicated support
- **Marketplace/Plugin System**: No third-party plugin architecture or app marketplace
- **Advanced Reporting Tools**: No dashboard builders or custom report designers

## Further Notes

### Competitive Positioning
Förena's success depends on executing significantly better usability than existing Swedish platforms. The open-source nature provides transparency and customization benefits, but the primary selling point must be that volunteers actually want to use Förena because it makes their lives easier.

### Implementation Phases
1. **Phase 1**: Core member management and basic event scheduling for sports clubs
2. **Phase 2**: Resource booking and HOA-specific workflows
3. **Phase 3**: Advanced communication features and analytics
4. **Phase 4**: Mobile optimization and automation features
5. **Phase 5**: Integration capabilities and advanced customization

### Success Metrics
- **Adoption Rate**: Number of Swedish organizations migrating from existing platforms
- **User Engagement**: Frequency of voluntary (non-admin) user logins and actions
- **Task Completion Time**: Measurable improvements in common workflow completion times
- **Support Burden**: Reduced support requests due to improved usability
- **Community Contribution**: Active development community and translation contributions

### Technical Sustainability
The open-source model requires building a sustainable development community. Priority should be given to clear documentation, modular architecture that enables community contributions, and coding standards that make the codebase approachable for volunteer developers familiar with TypeScript/React ecosystems.