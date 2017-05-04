package pong2;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.Point;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ComponentAdapter;
import java.awt.event.ComponentEvent;
import java.awt.event.MouseWheelEvent;
import java.awt.event.MouseWheelListener;
import java.sql.SQLException;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.Timer;

class PongPanel extends JPanel {

    private Point ballPoint = new Point(300, 400);
    final int ballDiameter = 20;
    int dx = 1, dy = 1;
    int left, right, top, bottom;
    final int padding = 80;
    private Timer ballUpdater;
    private Dimension paddleDimension = new Dimension(10,50);
    private final int paddleHorizontalShift = 20;
    private final int paddleX = padding + paddleHorizontalShift;
    private int paddleY = padding;
    private JLabel score;
    int points;
    boolean isGameOver;
    PongPanel() {

        setBackground(Color.BLUE);
        score = new JLabel("Your score is: " + points);
        score.setHorizontalTextPosition(JLabel.CENTER);
        score.setVerticalTextPosition(JLabel.CENTER);
        score.setForeground(Color.WHITE);
        add(score);
        
        addComponentListener(new ComponentAdapter() {
            public void componentResized(ComponentEvent e) {
                left = padding;
                right = getWidth() - padding;
                top = padding;
                bottom = getHeight() - padding;
                
                //setBorder();
            }
        });

        ballUpdater = new Timer(20, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent ae) {
                drawBall();
            }
        });
        ballUpdater.start();
        
        addMouseWheelListener( new MouseWheelListener() {
            @Override
            public void mouseWheelMoved(MouseWheelEvent mwe) {
                paddleY += mwe.getWheelRotation();
                repaint();
             }
        });
    }

    public boolean hitVerticalBounds() {
        return ballPoint.x <= left || ballPoint.x + ballDiameter >= right;
    }
    
    public void hitLeftBound() {
        if ( ballPoint.x <= left) {
            isGameOver = true;
            ballUpdater.stop();
        }
    }

    public boolean hitHorizontalBounds() {
        return ballPoint.y <= top || ballPoint.y + ballDiameter >= bottom;
    }
    
    public boolean hitPanel() {
       
        return ballPoint.x < paddleX + paddleDimension.width
              && ballPoint.y + ballDiameter >= paddleY 
              && ballPoint.y <= paddleY + paddleDimension.height;
    }

    public void updateBallPosition() {        
        if (hitVerticalBounds()) {
            dx = -dx;
            //System.out.println(ballPoint);
           // ballUpdater.setDelay(ballUpdater.getDelay() / 2);
        }
        if(hitPanel()) {
            dx = -dx;
            points++;
            score.setText("Your score is: " + points);
        }
        if (hitHorizontalBounds()) {
            dy = -dy;
           // System.out.println(ballPoint);
        }
        ballPoint.translate(dx, dy);
        
        hitLeftBound();

    }
    
    public void drawBall()
    {
        repaint();
     //   Graphics g = getGraphics();
        
//        if (!firstCall){
//        g.setColor(Color.BLACK);
//        g.fillOval(ballPoint.x, ballPoint.y, ballDiameter, ballDiameter);
//        }

    }
    
    @Override
    public void paint(Graphics g) {
        super.paint(g);               
        updateBallPosition();
        g.setColor(Color.WHITE);
        g.fillOval(ballPoint.x, ballPoint.y, ballDiameter, ballDiameter);
       
        g.fillRect(paddleX, paddleY,
                 paddleDimension.width, paddleDimension.height);
        
        g.setColor(Color.WHITE);
        g.drawRect(padding, padding, getWidth() - (padding * 2), getHeight() - (padding * 2));
       
    }
}

class PongApp extends JFrame {
    PongPanel pong;

    PongApp() {
        pong = new PongPanel();
        add(pong);
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setVisible(true);
    }
}

public class Main {

    public static void main(String[] args) throws SQLException {
       PongApp pa = new PongApp();
       while(!(pa.pong.isGameOver)) {
       }
       JOptionPane.showMessageDialog(null, "Game Over");
       DataBase db = new DataBase();
       db.checkStandings(pa.pong.points);
       System.exit(0);
       
       
        
    }
}

