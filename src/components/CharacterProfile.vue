<template>
	<div class="container-fuild" v-if="profiles.length > 0">
		<div class="row">
			<div v-for="(profile, i) in profiles" v-bind:key="i">  
				<div v-for="(character, j) in profile" v-bind:key="j" class="col-md-4 col-xs-12">
					<div class="row">
						<div class="col-md-12 col-xs-12">
							<div class="characterBox">
								<div class="row">
									<div class="col-md-6 col-xs-6">
										<img v-bind:src="character.image" v-bind:alt="getNameByGender(character.gender, character.name)">
									</div>
									<div class="col-md-6 col-xs-6">
										<div class="row">
											<div class="textLeft">
												<div class="col-md-12 col-xs-12">
													<a v-bind:href="character.url" class="">
														<span style="margin-left: -17px;font-size: x-large;font-weight: bold;">{{ getNameByGender(character.gender, character.name) }}</span>
													</a>
												</div>
												<div class="col-md-12 col-xs-12">
													<div class="characterBoxStatusColor">
														<span style="margin-left:-17px;">
															<span v-bind:class="getClass(character.status)" class="characterBoxStatusIcon"></span>
															<span class="characterBoxStatusText">{{ character.status }}</span> - <span>{{ character.species }}</span>
														</span>
													</div>	
												</div>
												<div class="col-md-12 col-xs-12">
													<div class="infoBlock">
														<div class="row">
															<div class="col-md-12 col-xs-12">
																<span class="">Last known location:</span>
															</div>
															<div class="col-md-12 col-xs-12">
																<a v-bind:href="character.location.url" class="">{{ character.location.name }}</a>
															</div>
														</div>
													</div>
												</div>
												<div class="col-md-12 col-xs-12">
													<div class="infoBlock">
														<div class="row">
															<div class="col-md-12 col-xs-12">
																<span class="">First seen in:</span>
															</div>
															<div class="col-md-12 col-xs-12">
																<a v-bind:href="character.url_last_episode" class="">{{ character.name_last_episode }}</a>
															</div>
														</div>
													</div>
												</div>
											</div>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div> 
		</div>
	</div>
</template>

<script>
export default {
  name: "CharacterProfile",
  props: {
    msg: String,
  },
  beforeMount: function(){
    this.loadCharactersProfiles();
  },
  data() {
	return {
		profiles: [],
		profilesTemp: [],
		allInfoLoaded: false,
		numbersOfCharacters: []
	};
  },
  filters: {},
  computed: { },
  methods: {
	generateRandomNumbers: function(){
		for(let i=0;i<25;i++){
			this.numbersOfCharacters.push(Math.floor((Math.random()*25) + 1))
		}
		return this.numbersOfCharacters;
	},
    loadCharactersProfiles: function(){
		const characters = this.generateRandomNumbers();

		const URL = 'https://rickandmortyapi.com/api/character/' + characters.toString();

		fetch(URL)
		.then(response => response.json())
		.then( data => {
			this.profilesTemp.push(data);
			this.getLastEpisodesByCharacter();
		});
    },
    getLastEpisodesByCharacter: function(){

      let profilesEpisodes = [];

      this.profilesTemp[0].forEach( element => {
        if(element.episode.length > 0){
          let lastEpisode = parseInt(element.episode[0].split("/")[5]);
          profilesEpisodes.push(parseInt(lastEpisode));
          fetch("https://rickandmortyapi.com/api/episode/" + lastEpisode)
          .then(response => response.json())
          .then(data => {
            element['name_last_episode'] = data.name;
            element['url_last_episode'] = data.url;
            if(this.profilesTemp[0].length === profilesEpisodes.length){
              this.allInfoLoaded = true;
            }
          });
        }
      });
      this.profiles = this.profilesTemp;
      //console.log('FINE COMPUTAZIONE: ', this.profiles[0]);
    },
    getNameByGender: function(gender, name){
      let nameCharacter = "";

      switch(gender.toLocaleLowerCase()){
        case 'female':
          nameCharacter = 'Mrs. ' + name;
          break;
        case 'male':
          nameCharacter = 'Mr. ' + name;
          break;
      }
      return nameCharacter;
    },
    getClass(status){
      let statusColor = "";

      switch(status.toLocaleLowerCase()){
        case 'unknown':
          statusColor = 'characterBoxStatusIconColorInActive';
          break;
        case 'alive':
          statusColor = 'characterBoxStatusIconColorActive';
          break;
        case 'dead':
          statusColor = 'characterBoxStatusIconColorDead';
          break;
      }
      return statusColor;
    }
  }
};
</script>

<style scoped src="../../src/assets/css/style.css"></style>
<style scoped src="../../src/assets/css/smartphone.css"></style>