<script>
    import {Client} from "./Client.js";
    import {
        Button,
        Card,
        CardBody,
        CardFooter,
        CardHeader,
        CardSubtitle,
        CardText,
        CardTitle,
        Collapse,
        Form, 
        FormGroup, 
        FormText, 
        Input, 
        Label
    } from 'sveltestrap';
    let endwork = 18;
    let startwork = 8;
    let nTime = 5;
    let arTimes;
    let isOpen = false;
    let modal = false;
    let nome;
    let telefone;

    let init = () => {
        generateTimes();
    }
    function generateTimes (){
      arTimes = Array.from(Array(nTime), () => new Object({time: numberRandomBtw(startwork,endwork), schedules: new Array() }));
        
    }

    let numberRandomBtw = (start, end) => {
        return Math.floor(Math.random() * (end - start + 1) + start)
    }

    let openModal = () => {
        modal = !modal;
    }
    let addSchedule = (t) => {
        if(nome && telefone){
            arTimes.forEach(e =>{
                if(e.time == t.time){
                    e.schedules.push(new Client(nome, telefone))
                }
            })
            console.log(arTimes);
        }new Client(nome, telefone)
    }
    init();
</script>

<h2>Barbearia Navalha Afiada</h2>

{#each arTimes as t}
    <Card class="mb-3">
        <CardHeader>
            <CardTitle>Hrs: {t.time}</CardTitle>
            <Button color="secondary" on:click={() => (isOpen = !isOpen)} class="mb-3">
                Show
            </Button>
          </CardHeader>
          <Collapse {isOpen}>
            <CardBody>
                <CardSubtitle>schedules</CardSubtitle>
                {#each t.schedules as schedule}
                    <CardText>
                        {schedule}
                    </CardText>
                {/each}
                <Button on:click={() => openModal()} color='success'>Cadastrar</Button>
                {#if modal}
                <div>
                    <Form>
                        <FormGroup>
                          <Label for="Nome">Nome</Label>
                          <Input plaintext bind:this={nome} />
                        </FormGroup>
                        <FormGroup>
                          <Label for="Telefone">Telefone: </Label>
                          <Input plaintext bind:this={telefone}/>
                        </FormGroup>
                        <Button on:click={() => addSchedule(t) } color='success'>Adicionar</Button>
                    </Form>
                </div>
                {/if}
            </CardBody>
          </Collapse>
    </Card>

{/each}