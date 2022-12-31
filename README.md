# How-to-create-account-related-contacts-in-Lightning-web-component-LWC

<p>Hey guys, today in this post we are going to learn about How to <strong>Create Account With Contact</strong> In LWC – Lightning Web Component Salesforce.</p>

<a href="https://www.w3web.net/create-account-with-contact-in-lwc/">Go to more details about this article__  <b><i>www.w3web.net</i></b></a><br/><br/>


<strong style="color:#ff0000;">Step 1) </strong> createConRelAcc.html [Lightning Web Component HTML]

  <template>
    <lightning-card title="How to Create Account With Contact In LWC" icon-name="standard:account">
        <div class="slds-grid slds-wrap">
            <div class="slds-col slds-col-size_12-of-12 slds-p-horizontal_medium slds-m-bottom_medium">
              <lightning-input label="Name"
                onchange={handleNameChange} 
                value={accountName}
                name="accountName"
                class="slds-m-bottom_x-small"></lightning-input>
              </div>

              
              <div class="slds-col slds-col-size_12-of-12 slds-p-horizontal_medium slds-m-bottom_medium slds-p-top_medium">
                <lightning-button label="Save"
                variant="brand"
                onclick={saveAction}></lightning-button>
              </div>
           </div>   
     </lightning-card>
</template>

<br/><br/>
<strong style="color:#ff0000;">Step 2) </strong> Create Lightning Web Component : createConRelAcc.js

SFDX:Lightning Web Component >> New >> createConRelAcc.js

createConRelAcc.js [LWC JavaScript File]

   import { LightningElement, track } from 'lwc';
 import { createRecord } from 'lightning/uiRecordApi';
 
 import { ShowToastEvent } from 'lightning/platformShowToastEvent';
 import accObj from '@salesforce/schema/Account';
 import accFld from '@salesforce/schema/Contact.AccountId';
 import nameFld from '@salesforce/schema/Account.Name';
 import conObj from '@salesforce/schema/Contact';
 import conNameFld from '@salesforce/schema/Contact.LastName';

export default class CreateConRelAcc extends LightningElement {

    @track accountName;
    @track accountPhone;
    @track accountId; 
    @track contactId; 
   
    handleNameChange(event){   
        if(event.target.name == 'accountName'){
            this.accountName = event.target.value;
        }  
         
       
       
    }


    
  
    saveAction() {
        const fields = {};
        fields[nameFld.fieldApiName] = this.accountName;
        const accRecordInput = { apiName: accObj.objectApiName, fields};
       
        createRecord(accRecordInput)
            .then(account => {
                this.accountId = account.id;
               
                this.dispatchEvent(
                    new ShowToastEvent({
                        title: 'Success',
                        message: 'Account created',
                        variant: 'success',
                    }),
                );
               
               
                const fields_Contact = {};
                fields_Contact[conNameFld.fieldApiName] = this.accountName + "'w3web contact";
                fields_Contact[accFld.fieldApiName] = this.accountId; 
                const recordInput_Contact = { apiName: conObj.objectApiName,
                                              fields : fields_Contact};
                 
                  createRecord(recordInput_Contact)
                    .then(contact => {
                        this.contactId = contact.id;
                        this.dispatchEvent(
                            new ShowToastEvent({
                                title: 'Success',
                                message: 'Contact created',
                                variant: 'success',
                            }),
                        );
 
                       this.accountName = ''; 
                    })
 
            })
            .catch(error => {
                this.dispatchEvent(
                    new ShowToastEvent({
                        title: 'Error creating record',
                        message: error.body.message,
                        variant: 'error',
                    }),
                );
            });
    }


}



<br/><br/>
Create Lightning Web Component Meta XML
<strong style="color:#ff0000;">Step 3) </strong> Create Lightning Web Component : createConRelAcc.js-meta.xml

   <?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>55.0</apiVersion>
    <isExposed>true</isExposed>
    <targets>
    <target>lightning__AppPage</target>
    <target>lightning__RecordPage</target>
    <target>lightning__HomePage</target>
  </targets>
</LightningComponentBundle>

<br/><br/>
<a href="https://www.w3web.net/create-account-with-contact-in-lwc/">Go to more details about this article__  <b><i>www.w3web.net</i></b></a><br/><br/>
