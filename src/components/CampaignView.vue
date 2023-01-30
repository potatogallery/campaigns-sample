<template>
  <div>
    <h2>Registration</h2>
    <form @submit.prevent="save">
  <div class="form-group">
    <label>Campaign ID</label>
    <input type="text" v-model="campaign.campaignid" class="form-control" placeholder="Enter Campaign ID">
  </div>
  <div class="form-group">
    <label>Campaign Name</label>
    <input type="text" v-model="campaign.name" class="form-control"  placeholder="Enter Campaign Name">
  </div>
  <div class="form-group">
    <label>Campaign Type</label>
    <input type="text" v-model="campaign.campaigntype" class="form-control"  placeholder="Enter Campaign Type">
  </div>
  <button type="submit" class="btn btn-primary">Save</button>
</form>
<br>
    <h2>Campaign View</h2>
    <table class="table table-dark">
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Campaign ID</th>
      <th scope="col">Name</th>
      <th scope="col">Type</th>
      <th scope="col">Option</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="campaign in result" v-bind:key="campaign.id">
      <td>{{campaign.id}}</td>
      <td>{{campaign.campaignid}}</td>
      <td>{{campaign.name}}</td>
      <td>{{campaign.campaigntype}}</td>
      <td>
      <button type="button" class="btn btn-warning" @click="edit(campaign)">Edit</button>
      <button type="button" class="btn btn-danger" @click="remove(campaign)">Delete</button>
      </td>
    </tr>
  </tbody>
</table>
</div>

</template>

<script>
import Vue from 'vue'
import axios from 'axios'
Vue.use(axios)

export default {
  name: 'CampaignView',
  data () {
    return {
      result: {},
      campaign: {
        campaignid: '',
        name: '',
        campaigntype: ''
      }
    }
  },
  created () {
    this.CampaignLoad()
  },
  mounted () {
    console.log('mounted() called...')
  },
  methods: {
    /*
    *
    Landing Page, /campaign API Called from the laravel project(Backend)
    *
    */
    CampaignLoad () {
      var page = 'http://localhost:8000/api/campaign'
      axios.get(page)
        .then(
          ({data}) => {
            console.log(data)
            this.result = data
          }
        )
    },
    /*
    *
    Save button pressed for updating or creating campaign, methods of saveData and updateData are used
    *
    */
    save () {
      if (this.campaign.id !== '') {
        this.saveData()
      } else {
        this.updateData()
      }
    },
    /*
    *
    SaveData Method, /save API Called from the laravel project(Backend)
    *
    */
    saveData () {
      axios.post('http://localhost:8000/api/save', this.campaign)
        .then(
          ({data}) => {
            alert('Campaign Created')
          }
        )
    },
    /*
    *
    Update Method, /update{id} API Called from the laravel project(Backend)
    *
    */
    edit (campaign) {
      this.campaign = campaign
    },
    updateData () {
      var editrecords = 'http://localhost:8000/api/update' + this.campaign.id
      axios.put(editrecords, this.campaign)
        .then(
          ({data}) => {
            this.campaign.campaignid = ''
            this.campaign.name = ''
            this.campaign.campaigntype = ''
            alert('Campaign Updated!')
            this.CampaignLoad()
          }
        )
    },
    /*
    *
    Delete Method, API Called from the laravel project(Backend)
    *
    */
    remove (campaign) {
      var url = 'http://localhost:8000/api/delete/' + campaign.id
      axios.delete(url)
      alert('Campaign Deleted')
      this.CampaignLoad()
    }
  }
}
</script>
