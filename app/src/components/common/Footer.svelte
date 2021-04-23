<script>
    import { Container, Row, Col, MaterialApp } from 'svelte-materialify';
    import GoogleNewsLab from '../../img/GoogleNewsLab.svelte'
    import ViT from '../../img/ViT.svelte'
    import UdG from '../../img/UdG.svelte'
    import Tweet from './Tweet.svelte'
</script>
<div class='fs footer'>
    <div class='col'>
    <Container>
        <h3>Like it? Share it!
            <Tweet
                tweet=''
                url=''
            />
        </h3>
        <Row>
            <Col cols={12} sm={12} md={8}>
                
                
                <h4>About the project</h4>
                <p><slot name='about'></slot></p>
                <h4>The project</h4>
                <p><slot name='data'></slot></p>
            </Col>
            <Col cols={12} sm={12} md={4}>
                <h4>Made by</h4>
                <p><slot name='team'></slot></p>
                <p><b>Google News Lab:</b> <a href='https://simonrogers.net/'>Simon Rogers</a> and <a href='http://albertocairo.com/'>Alberto Cairo</a></p>
                <p><b>MVTEC mentors: </b> Mentor, mentor, and mentor</p>
            </Col>
        </Row>
        <p>This project was designed and developed in the <a href='http://www.mastervisualtoolsudg.com/'>Master’s program in Visual Tools to Empower Citizens (MVTEC)</a> organized by <a href='https://www.fundacioudg.org/'>Fundació UdG</a> and the <a href='https://www.fundaciovit.org/'>ViT Foundation</a></p>
        <Row>
            <Col cols={12} sm={4}><GoogleNewsLab /></Col>
            <Col cols={12} sm={2}><ViT /></Col>
            <Col cols={12} sm={3}><UdG /></Col>
        </Row>
    </Container>
    </div>
</div>
	
<style>
    .footer {
        background-color:#284385;
        color:#FFF;
        padding-top:5rem;
    }
    .footer p {
        font-size: 1rem;
    }

    b {
        font-weight: 700;
    }

</style>