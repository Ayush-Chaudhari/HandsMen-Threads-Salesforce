# HandsMen Threads - Salesforce Data Management System

A comprehensive Salesforce implementation for fashion industry data management and customer relationship enhancement.

## 📋 Project Overview

HandsMen Threads is a dynamic fashion organization's Salesforce project designed to revolutionize data management and enhance customer relations. The project builds a robust data model to store business data while ensuring seamless information flow across the organization.

## ✨ Key Features

### Automated Business Processes
- **Order Confirmations**: Automated email notifications post-order confirmation
- **Dynamic Loyalty Program**: Customer loyalty status updates based on purchase history
- **Proactive Stock Alerts**: Automatic inventory notifications when stock drops below 5 units
- **Scheduled Bulk Processing**: Daily midnight processing for bulk orders and financial record updates

### Data Integrity
- Real-time data validation through UI
- Comprehensive validation rules
- Automated data quality checks

## 🎯 Learning Objectives

This project covers:
1. **Data Modelling** - Custom objects and relationships
2. **Data Quality** - Validation rules and integrity checks
3. **Lightning App Builder** - Modern UI development
4. **Record Triggered Flows** - Automated business processes
5. **Apex and Apex Triggers** - Custom business logic
6. **Asynchronous Processing** - Batch jobs and scheduled operations

## 🏗️ Architecture

### Data Model

#### Custom Objects

| Object | Purpose | Key Fields |
|--------|---------|------------|
| **HandsMen Customer** | Customer information | Name, Email, Phone, Loyalty_Status__c, Total_Purchases__c |
| **HandsMen Product** | Product catalog | Name, SKU, Price, Stock_Quantity__c |
| **HandsMen Order** | Customer orders | Order_Number, Status, Quantity__c, Total_Amount__c |
| **Inventory** | Inventory tracking | Auto Number, Warehouse, Stock_Quantity__c |
| **Marketing Campaign** | Campaign management | Campaign_Name, Start_Date, End_Date |

#### Relationships
- **Marketing Campaign** ↔ **HandsMen Customer** (Lookup)
- **HandsMen Product** ↔ **HandsMen Order** (Lookup)
- **HandsMen Order** ↔ **HandsMen Customer** (Lookup)
- **Inventory** ↔ **HandsMen Product** (Master-Detail)

### Formula Fields
- **Stock_Status__c**: `IF(Stock_Quantity__c > 10, "Available", "Low Stock")`
- **Full_Name__c**: `FirstName__c + " " + LastName__c`

### Validation Rules
- **Order Total Amount**: `Total_Amount__c <= 0`
- **Inventory Stock**: `Stock_Quantity__c <= 0`
- **Customer Email**: `NOT CONTAINS(Email, "@gmail.com")`

## 🚀 Setup Instructions

### Prerequisites
- Salesforce Developer Account
- System Administrator access

### Installation Steps

1. **Create Developer Account**
   - Visit [developer.salesforce.com/signup](https://developer.salesforce.com/signup)
   - Fill registration details with your information

2. **Deploy Custom Objects**
   - Navigate to Setup → Object Manager → Create → Custom Object
   - Create all custom objects as per specifications

3. **Configure Relationships**
   - Set up lookup and master-detail relationships between objects

4. **Create Custom Fields**
   - Add required fields to each object
   - Configure formula fields and validation rules

5. **Set Up Security**
   - Create custom profiles and permission sets
   - Configure roles and user access

6. **Deploy Automation**
   - Create email templates
   - Set up flows for automated processes
   - Deploy Apex classes and triggers

## 📧 Email Templates

### Order Confirmation
```html
<p>Dear {!Order__c.Customer__c},</p>
<p>Your order #{!Order__c.Name} has been confirmed!</p>
<p>Thank you for shopping with us.</p>
<p>Best Regards,</p>
<p>Sales Team</p>
```

### Additional Templates
- **Low Stock Alert**: Warehouse team notifications
- **Loyalty Program Email**: Customer loyalty updates

## 🔄 Automation Components

### Flows
1. **Order Confirmation Flow** - Triggered when order status changes to "Confirmed"
2. **Stock Alert Flow** - Triggered when inventory drops below 5 units
3. **Loyalty Status Update Flow** - Scheduled daily to update customer loyalty status

### Apex Components

#### OrderTriggerHandler.cls
```apex
public class OrderTriggerHandler {
    public static void validateOrderQuantity(List<HandsMen_Order__c> orderList) {
        for (HandsMen_Order__c order : orderList) {
            if (order.Status__c == 'Confirmed') {
                if (order.Quantity__c == null || order.Quantity__c <= 500) {
                    order.Quantity__c.addError('For Status "Confirmed", Quantity must be more than 500.');
                }
            }
            // Additional validation logic...
        }
    }
}
```

#### InventoryBatchJob.cls
```apex
global class InventoryBatchJob implements Database.Batchable<SObject>, Schedulable {
    global Database.QueryLocator start(Database.BatchableContext BC) {
        return Database.getQueryLocator(
            'SELECT Id, Stock_Quantity__c FROM Product__c WHERE Stock_Quantity__c < 10'
        );
    }
    
    global void execute(Database.BatchableContext BC, List<SObject> records) {
        // Batch processing logic for inventory updates
    }
    
    global void finish(Database.BatchableContext BC) {
        System.debug('Inventory Sync Completed');
    }
}
```

## 📊 Project Phases

### Phase 1: Architecture & Planning
- Define objects, fields, and relationships
- Establish validation rules and automation strategy
- Design email templates

### Phase 2: Development
- Create objects and fields
- Implement automation (flows, triggers)
- Set up security and sharing rules
- Develop batch jobs

### Phase 3: Testing & QA
- Unit testing of objects and automation
- End-to-end testing with sample data
- Performance and security testing

### Phase 4: Deployment & Training
- Production deployment
- User training
- Post-go-live support

## 🔐 Security Model

### Profiles
- **Platform 1**: Custom profile for platform users
- **System Administrator**: Full access

### Roles
- **Sales**: Sales team access
- **Inventory**: Inventory management access
- **Marketing**: Marketing campaign access

### Permission Sets
- **Permission_Platform_1**: Additional permissions for platform users

## 📱 Lightning App Configuration

**HandsMen Threads App** includes:
- All custom objects
- Standard objects (Account, Contact)
- Reports and Dashboards
- Configured for System Administrator profile

## 🛠️ Development Tools

- **Salesforce Developer Console**: Apex development
- **Lightning App Builder**: UI customization
- **Flow Builder**: Process automation
- **Setup**: Configuration management

## 📈 Monitoring & Maintenance

### Scheduled Jobs
- Daily inventory sync at midnight
- Batch processing monitoring
- Email delivery tracking

### Performance Optimization
- Bulk data processing
- Efficient SOQL queries
- Proper governor limit handling

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📞 Support

For support and questions:
- Create an issue in this repository
- Contact the development team
- Check Salesforce documentation

## 🚀 Future Enhancements

- Integration with external payment systems
- Advanced analytics and reporting
- Mobile app development
- AI-powered recommendations

## Project Structure

All source files are located in the following directory:
force-app/main/default
This folder contains all Salesforce metadata components such as Apex classes, Lightning components, triggers, etc.



---

**Built with ❤️ for the Fashion Industry**
